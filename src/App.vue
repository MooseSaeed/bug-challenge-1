<template>
  <div>
    <figure>
      <img src="/vueschool-logo.png" alt="VueSchool Logo" />
    </figure>
    <h1>Wise Quotes</h1>
    <button @click="sortQuotes">Sort rating by: {{ sortOrder }}</button>
    <button @click="loadMore" :disabled="!quotes.length">Load more</button>
    <TransitionGroup name="list" tag="ul">
      <li v-for="quote in quotes" :key="quote._id">
        <span>Rating: {{ quote.rating }}</span>
        <blockquote>
          <p>{{ quote.content }}</p>
          <cite>- {{ quote.author }}</cite>
        </blockquote>
      </li>
    </TransitionGroup>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const quotes = ref([]);
const fetchQuotes = async () => {
  const response = await fetch('https://api.quotable.io/quotes?limit=10');
  const data = await response.json();
  quotes.value = data.results.map((quote) => ({
    ...quote,
    rating: Math.floor(Math.random() * 20) + 1,
  }));
};

onMounted(() => {
  fetchQuotes();
});

const sortOrder = ref('desc');

const sortQuotes = () => {
  if (sortOrder.value === 'desc') {
    quotes.value.sort((a, b) => b.rating - a.rating);
    sortOrder.value = 'asc';
  } else {
    quotes.value.sort((a, b) => a.rating - b.rating);
    sortOrder.value = 'desc';
  }
};

const loadMore = () => {
  fetchQuotes();
};
</script>

<style>
figure {
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 200px;
  height: 45px;
  background-color: white;
  padding: 10px;
  border-radius: 30px;
  overflow: hidden;
  margin: 0 auto;
}

img {
  width: 100%;
}

h1 {
  color: #fc6c94;
}

button {
  margin-bottom: 10px;
  background-color: #00bda7;
  display: block;
  padding-left: auto;
  padding-right: auto;
}

button:hover {
  background-color: #00b49c;
}

ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 15px;
  place-items: center;
  justify-items: center;
  width: 100%;
}

li {
  color: white;
  background-color: #00c3fc;
  overflow: hidden;
  border-radius: 1rem;
  max-width: 600px;
  border-color: white;
  border: 1px;
  position: relative;
}

span {
  display: block;
  background-color: #342c8c;
  padding-top: 10px;
  padding-bottom: 10px;
}

blockquote {
  padding-top: 5px;
  padding-bottom: 10px;
}

p {
  font-weight: bold;
}

.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.list-leave-active {
  position: absolute;
}
</style>
