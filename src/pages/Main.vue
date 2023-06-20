<template>
  <main>
    <!-- <section class="py-5 text-center container">
      <div class="row py-lg-5">
        <div class="col-lg-6 col-md-8 mx-auto">
          <h1 class="fw-light">Album example</h1>
          <p class="lead text-body-secondary">
            Something short and leading about the collection below—its contents,
            the creator, etc. Make it short and sweet, but not too short so
            folks don’t simply skip over it entirely.
          </p>
          <p>
            <a href="#" class="btn btn-primary my-2">Main call to action</a>
            <a href="#" class="btn btn-secondary my-2">Secondary action</a>
          </p>
        </div>
      </div>
    </section> -->

    <div class="album py-5 bg-body-tertiary">
      <div class="container">
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
          <div
            class="col col-md-4"
            v-for="product in products"
            :key="product.id"
          >
            <div class="card shadow-sm mb-4">
              <img :src="product.image" height="180" />
              <div class="card-body">
                <p class="card-text">
                  {{ product.title }}
                </p>
                <div class="d-flex justify-content-between align-items-center">
                  <div class="btn-group">
                    <button
                      type="button"
                      class="btn btn-sm btn-outline-secondary"
                      @click="like(product.id)"
                    >
                      Like
                    </button>
                  </div>
                  <small class="text-body-secondary text-muted"
                    >{{ product.likes }} likes</small
                  >
                </div>
              </div>
            </div>
          </div>

          <!-- <div class="col">
            <div class="card shadow-sm">
              <svg
                class="bd-placeholder-img card-img-top"
                width="100%"
                height="225"
                xmlns="http://www.w3.org/2000/svg"
                role="img"
                aria-label="Placeholder: Thumbnail"
                preserveAspectRatio="xMidYMid slice"
                focusable="false"
              >
                <title>Placeholder</title>
                <rect width="100%" height="100%" fill="#55595c" />
                <text x="50%" y="50%" fill="#eceeef" dy=".3em">Thumbnail</text>
              </svg>
              <div class="card-body">
                <p class="card-text">
                  This is a wider card with supporting text below as a natural
                  lead-in to additional content. This content is a little bit
                  longer.
                </p>
                <div class="d-flex justify-content-between align-items-center">
                  <div class="btn-group">
                    <button
                      type="button"
                      class="btn btn-sm btn-outline-secondary"
                    >
                      View
                    </button>
                    <button
                      type="button"
                      class="btn btn-sm btn-outline-secondary"
                    >
                      Edit
                    </button>
                  </div>
                  <small class="text-body-secondary">9 mins</small>
                </div>
              </div>
            </div>
          </div> -->
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { ref, onMounted } from "vue";
import { Product } from "@/interfaces/product";
export default {
  name: "Main",
  setup() {
    const products = ref([] as Product[]);

    onMounted(async () => {
      const response = await fetch("http://localhost:8000/api/products");

      products.value = await response.json();
    });

    const like = async (id: number) => {
      await fetch(`http://localhost:8000/api/products/${id}/like`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
      });

      products.value = products.value.map((p: Product) => {
        if (p.id === id) {
          p.likes++;
        }

        return p;
      });
    };

    return {
      products,
      like,
    };
  },
};
</script>
