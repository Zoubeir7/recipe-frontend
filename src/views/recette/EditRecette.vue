<script setup>
import { ref, onMounted } from "vue";
import { useRecipeStore } from "@store/recipeStore";
import { useCategoryStore } from "@store/categoryStore";
import { useRouter } from "vue-router";

const recipeStore = useRecipeStore();
const categoryStore = useCategoryStore();
const router = useRouter();
const newRecipe = ref({ title: '', type: '', ingredients: '', categoryId: '' });

const recipeId = router.currentRoute.value.params.id;

onMounted(async () => {
  if (recipeId) {
    const recette = await recipeStore.getRecipeById(recipeId);
    newRecipe.value = recette;
  }

  await categoryStore.loadCategoriesFromAPI();
});


const saveRecipe = async () => {
  await recipeStore.editRecipe(newRecipe.value);
  router.push({ name: 'recette' });
};
</script>

<template>
  <div class="container mt-5">
    <div class="p-4 bg-light rounded shadow-sm">
      <div class="input-group mb-4">
        <span class="input-group-text bg-warning text-dark fw-bold">
          <i class="fas fa-pen"></i>&nbsp;{{ t('recipes.edit_form.tilte') }}
        </span>
        <input type="text" class="form-control" placeholder="Titre de la recette" v-model="newRecipe.title" />
      </div>

      <div class="input-group mb-4">
        <span class="input-group-text bg-warning text-dark fw-bold">
          <i class="fas fa-utensils"></i>&nbsp;{{ t('recipes.edit_form.type') }}
        </span>
        <select class="form-select" v-model="newRecipe.type">
          <option value="Entrée">{{ t('recipes.edit_form.option_Entrée') }}</option>
          <option value="Plat">{{ t('recipes.edit_form.option_plat') }}</option>
          <option value="Dessert">{{ t('recipes.edit_form.option_dessert') }}</option>
        </select>
      </div>

      <div class="input-group mb-4">
        <span class="input-group-text bg-warning text-dark fw-bold">
          <i class="fas fa-carrot"></i>&nbsp;{{ t('recipes.edit_form.ingredients') }}
        </span>
        <input type="text" class="form-control" placeholder="Liste des ingrédients (séparés par des virgules)"
          v-model="newRecipe.ingredients" />
      </div>

      <div class="input-group mb-4">
        <span class="input-group-text bg-warning text-dark fw-bold">
          <i class="fas fa-tags"></i>&nbsp;{{ t('recipes.edit_form.categorie') }}
        </span>
        <select class="form-select" v-model="newRecipe.categoryId">
          <option v-for="category in categoryStore.categories" :key="category.id" :value="category.id">
            {{ category.name }}
          </option>
        </select>
      </div>

      <button class="btn btn-warning w-100 fw-bold" @click="saveRecipe">
        <i class="fas fa-save"></i> {{ t('recipes.edit_form.button') }}
      </button>
    </div>
  </div>
</template>

<style scoped></style>
