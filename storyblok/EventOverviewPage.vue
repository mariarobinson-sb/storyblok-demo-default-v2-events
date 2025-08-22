<script setup>
defineProps({ blok: Object });

const { slug } = useRoute().params;
let language = 'default';

if (slug) {
  language = await getLanguage(slug);
}

const searchTerm = ref('');
const checkedCategory = ref('');

const filterQuery = computed(() => {
  const query = {};

  if (checkedCategory.value) {
    query.categories = {
      in: checkedCategory.value,
    };
  }

  return query;
});

const storyblokApi = useStoryblokApi();

const loading = ref(true);

const events = ref(null);

const fetchEvents = async () => {
  loading.value = true;
  events.value = null;
  const { data } = await storyblokApi.get('cdn/stories/', {
    version: getVersion(),
    starts_with: 'events',
    language,
    fallback_lang: 'default',
    search_term: searchTerm.value,
    filter_query: filterQuery.value,
    resolve_relations: 'default-page.categories',
  });
  events.value = data.stories.filter(story => story.is_startpage !== true);
  loading.value = false;
};

fetchEvents();

const categories = ref(null);

const getCategories = async () => {
  const { data } = await storyblokApi.get('cdn/stories/', {
    version: getVersion(),
    starts_with: 'categories',
  });
  categories.value = data.stories.filter(story => story.is_startpage !== true);
};

getCategories();

const resetCategories = () => {
  checkedCategory.value = '';
  fetchEvents();
};

watch(searchTerm, () => {
  fetchEvents();
});

watch(checkedCategory, () => {
  fetchEvents();
});

const gridClasses = computed(() => getGridClasses());
</script>

<template>
  <section v-editable="blok" class="container">
    <header>
      <H1Headline
        v-if="blok.headline"
      >
        {{ blok.headline }}
      </H1Headline>
      <nav>
        <div class="mb-12 flex justify-center">
          <label for="search" class="sr-only">Search</label>
          <input
            id="search"
            v-model="searchTerm"
            type="search"
            name="search"
            placeholder="Search for anything"
            class="border-dark rounded-lg border-2 px-12 py-4 text-xl focus:outline-none"
            @keypress.enter="fetchEvents()"
          />
        </div>
        <div class="border-medium mb-12 flex flex-col rounded-lg border p-1 lg:flex-row">
          <button
            class="w-full cursor-pointer rounded-md px-6 py-3 text-center text-lg"
            :class=" !checkedCategory ? 'bg-primary-dark text-white' : 'text-primary-dark'"
            @click.prevent="resetCategories()"
          >
            All
          </button>
          <label
            v-for="category in categories"
            :key="category.uuid"
            :for="category.uuid"
            class="w-full cursor-pointer rounded-md px-6 py-3 text-center text-lg text-primary-dark"
            :class=" checkedCategory === category.uuid ? 'bg-primary-dark text-white' : 'text-primary-dark'"
          >
            <input
              :id="category.uuid"
              v-model="checkedCategory"
              type="radio"
              :name="category.uuid"
              :value="category.uuid"
              class="sr-only"
            />
            <span>{{ category.name }}</span>
          </label>
        </div>
      </nav>
    </header>
    <main class="pb-24">
      <section
        v-if="!loading && events.length"
        :class="gridClasses"
      >
        <EventCardVertical
          v-for="event in events"
          :key="event.uuid"
          :event="event.content"
          :slug="event.full_slug"
        />
      </section>

      <section v-else-if="!loading && !events.length">
        Unfortunately, no events matched your criteria.
      </section>
    </main>
  </section>
</template>
