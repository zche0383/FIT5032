<!-- JSONLab.vue -->

<template>
  <div class="json-lab">
    <h1>🗄️ JSON Data & Vue Directives Lab</h1>

    <section class="lab-section">
      <h2>📚 Working with JSON Arrays</h2>
      <p>Our <code>authors.json</code> contains an array of author objects.</p>

      <h3>Iterating through Arrays</h3>
      <ul>
        <li
          v-for="author in authors"
          :key="author.id"
          :class="{ highlight: author.name === 'George Orwell' }"
          :style="author.name === 'George Orwell' ? { fontWeight: '700', color: '#065f46' } : {}"
          :title="author.name === 'George Orwell' ? 'Highlighted author: George Orwell' : author.name"
        >
          {{ author.name }} ({{ author.birthYear }})
        </li>
      </ul>

      <h3>Filtering Arrays</h3>
      <p>Authors born after 1850:</p>
      <ul>
        <li v-for="author in modernAuthors" :key="author.id">
          {{ author.name }} ({{ author.birthYear }})
        </li>
      </ul>

      <h3>Mapping Arrays</h3>
      <p>Famous works:</p>
      <ul>
        <li v-for="work in allFamousWorks" :key="work">
          {{ work }}
        </li>
      </ul>

      <h3>Finding in Arrays</h3>
      <p>Finding by property: {{ orwell?.name }}</p>

      <h3>Nested Arrays/Objects</h3>
      <p>{{ austen?.name }}'s works:</p>
      <ul>
        <li v-for="work in austenWorks" :key="work.title">
          {{ work.title }} ({{ work.year }})
        </li>
      </ul>
    </section>

    <section class="lab-section">
      <h2>🏢 Working with JSON Objects</h2>
      <p>Our <code>bookstores.json</code> is a JSON object.</p>

      <h3>Accessing Properties</h3>
      <p>Company: {{ companyName }}</p>
      <p>Total Stores: {{ totalStores }}</p>

      <h3>Iterating Object Properties</h3>
      <p>Store Types:</p>
      <ul>
        <li v-for="([type, count]) in storeTypes" :key="type">
          {{ type }}: {{ count }} stores
        </li>
      </ul>

      <h3>Nested Objects</h3>
      <p>Opening Hours:</p>
      <ul>
        <li v-for="([day, hours]) in openingHours" :key="day">
          {{ day }}: {{ hours.open }} - {{ hours.close }}
        </li>
      </ul>

      <h3>Working with Arrays in Objects</h3>
      <p>We operate in: {{ operatingCountries }}</p>
      <p>Our #1 seller: {{ topSeller }}</p>
    </section>

    <section class="lab-section">
      <h2>v-if & v-else</h2>
      <p>Toggle visibility based on a condition.</p>
      <button @click="showMessage = !showMessage">Toggle Message</button>
      <p v-if="showMessage" class="message success">
        ✨ You're a Vue superstar! ✨
      </p>
      <p v-else class="message">Click the button to see a message.</p>
    </section>

    <section class="lab-section">
      <h2>Attribute, Class and Style Binding with <code>v-bind</code></h2>
      <p>Bind a dynamic image and an external link using attribute binding.</p>
      <div class="binding-example">
        <img :src="imageUrl" alt="Dynamic Image" class="dynamic-image" />
        <a :href="documentationUrl" target="_blank" rel="noreferrer">
          Vue v-bind documentation
        </a>
      </div>

      <h3>Class Binding</h3>
      <button :class="{ active: isActive }" @click="isActive = !isActive">
        Toggle Active Class
      </button>

      <h3>Style Binding</h3>
      <div
        :class="{ highlight: isHighlighted }"
        :style="{ color: textColor, fontSize: fontSize + 'px' }"
      >
        This text is styled using v-bind.
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, computed } from "vue"
import authors from "../assets/json/authors.json"
import bookstores from "../assets/json/bookstores.json"
import logoUrl from "../assets/logo.svg"

const showMessage = ref(false)
const isHighlighted = ref(true)
const isActive = ref(true)
const textColor = ref("blue")
const fontSize = ref(18)
const imageUrl = logoUrl
const documentationUrl =
  "https://vuejs.org/guide/essentials/template-syntax.html#v-bind"

const modernAuthors = computed(() =>
  authors.filter((author) => author.birthYear > 1850)
)

const allFamousWorks = computed(() =>
  authors.flatMap((author) => author.famousWorks.map((work) => work.title))
)

const orwell = computed(() =>
  authors.find((author) => author.name === "George Orwell")
)

const austen = computed(() =>
  authors.find((author) => author.name === "Jane Austen")
)

const austenWorks = computed(() => austen.value?.famousWorks ?? [])

const companyName = computed(() => bookstores.name)
const totalStores = computed(() => bookstores.totalStores)
const storeTypes = computed(() => Object.entries(bookstores.storeTypes))
const openingHours = computed(() => Object.entries(bookstores.openingHours))
const operatingCountries = computed(() => bookstores.countries.join(", "))
const topSeller = computed(() => bookstores.topSellers?.[0] ?? "")
</script>

<style scoped>
.json-lab {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  max-width: 80vw;
  margin: 0 auto;
  padding: 20px;
  background-color: #f4f4f4;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1,
h2 {
  color: #333;
}

h1 {
  text-align: center;
}

.lab-section {
  background-color: white;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.message {
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
}

.success {
  background-color: #e7faf3;
  color: #42b883;
  border: 1px solid #42b883;
}

.highlight {
  background-color: #42b883;
}

.binding-example {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 1rem;
  margin-bottom: 16px;
}

.dynamic-image {
  width: 120px;
  border-radius: 10px;
  border: 1px solid #ccc;
}

button.active {
  background-color: #42b883;
  color: white;
  border: none;
}

code {
  background-color: #e0e0e0;
  padding: 2px 5px;
  border-radius: 4px;
  font-family: "Courier New", Courier, monospace;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #f0f0f0;
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
}
</style>
