<script setup>
import { useRouter } from "vue-router";
import { ref, onMounted } from "vue";
import { Modal } from "bootstrap";
import { useI18n } from "vue-i18n";

const { locale } = useI18n();
const t = useI18n();

import { useRecipeStore } from "@store/recipeStore";

const store = useRecipeStore();
const recettetModal = ref(null);
const route = useRouter();

const recipes = store.recipes;

onMounted(() => {
  const modalElement = recettetModal.value;
  const bootstrapModal = new Modal(modalElement);
  bootstrapModal.show();
});
</script>

<template>
  <div class="container-fluid">
    <div
      class="modal fade"
      id="modalId"
      ref="recettetModal"
      tabindex="-1"
      data-bs-backdrop="static"
      data-bs-keyboard="false"
      role="dialog"
      aria-labelledby="modalTitleId"
      aria-hidden="false"
    >
      <div
        class="modal-dialog modal-dialog-scrollable modal-dialog-centered"
        role="document"
      >
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="text-center fw-bold" id="modalTitleId">
              {{ $t("recipes.view_form.champ_detail") }}
            </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
              @click="route.push({ name: 'recette' })"
            ></button>
          </div>
          <div class="modal-body">
            <p v-if="recipes[store.val].id">
              <span class="fw-bold">N° : </span>{{ recipes[store.val].id }}
            </p>
            <p v-if="recipes[store.val].title">
              <span class="fw-bold"
                >{{ $t("recipes.view_form.champ_titre") }} : </span
              >{{ recipes[store.val].title }}
            </p>
            <p v-if="recipes[store.val].type">
              <span class="fw-bold"
                >{{ $t("recipes.view_form.champ_type") }} : </span
              >{{ recipes[store.val].type }}
            </p>
            <div v-if="recipes[store.val].ingredients">
              <thead>
                <th colspan="{{ recipes.ingredients.length }}">
                  {{ $t("recipes.view_form.champ_ingredient") }}
                </th>
              </thead>
              <tbody>
                <tr
                  v-for="(ingredient, val) in recipes[store.val].ingredients"
                  :key="val"
                >
                  <td class="text-success">- {{ ingredient }}</td>
                </tr>
              </tbody>
            </div>
          </div>
          <div class="modal-footer d-flex justify-content-between">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
              @click="route.push({ name: 'recette' })"
            >
              {{ $t("recipes.view_form.button_close") }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
