<template>
  <div>
    <!-- 공개여부(open) 값에 따라서 미리보기(false)/상세보기(true)로 구분 -->
    <h4 v-if="recipe_data.open === true">{{ $t("title.viewRecipe") }}</h4>
    <h4 v-else>{{ $t("title.previewRecipe") }}</h4>

    <!-- 작성자 정보 -->
    <p>
      {{
        recipe_data.writer.nickname
          ? recipe_data.writer.nickname
          : recipe_data.writer.snsId.substr(0, 5) + "..."
      }}
    </p>

    <!--대표이미지 -->
    <p>
      <img
        :src="`${$API_SERVER}/file/download?fileId=${recipe_data.mainImgId}`"
        width="200"
        height="150"
      />
    </p>

    <!-- 제목/ 부제목 -->
    <p>{{ recipe_data.title }}</p>
    <p>{{ recipe_data.subTitle }}</p>

    <!-- 이유시기 / 소요시간-->
    <p>
      {{ $t("option.period[" + recipe_data.period + "]") }} /
      {{ $t("option.timeTaken[" + recipe_data.timeTaken + "]") }}
    </p>

    <!-- 재료소개 -->
    <p>{{ $t("content.listOfIngredient") }}</p>
    <p>
      {{ $t("content.quantity") }} : {{ recipe_data.quantity }} {{ $t("content.times") }}
    </p>
    <p>
      <span v-for="(ing, idx) in recipe_data.ingredients" :key="idx">
        <span>{{ $t("ingredient." + ing.key) }} : {{ ing.volume }} </span>
        <span v-if="ing.key !== 'WATER'">g</span>
        <span v-else>ml</span><br v-if="idx % 2 === 1"
      /></span>
    </p>

    <!-- 조리순서 -->
    <p>{{ $t("content.cookingOrder") }}</p>
    <div v-for="(order, idx) in recipe_data.cookingOrder" :key="idx">
      <p>{{ order.contentsNo }} : {{ order.contents }}</p>
      <p>
        <img
          :src="`${$API_SERVER}/file/download?fileId=${order.imgId}`"
          width="200"
          height="150"
        />
      </p>
    </div>

    <!-- Tip -->
    <div v-if="recipe_data.tips">
      <p>{{ $t("content.tip") }}</p>
      <div v-for="(tip, idx) in recipe_data.tips" :key="idx">
        <p>{{ tip.text }}</p>
      </div>
    </div>

    <!-- clip movie -->
    <!-- clip url이 정상적인 youtube url인지 확인/
          - 정상 : iframe으로  display
          - 비정상 : broken movive image로 display -->
    <div v-if="recipe_data.clipLink">
      <iframe id="clip" width="200" height="150" :src="recipe_data.clipLink"></iframe>
      <!-- {{ recipe_data.clipLink }} -->
    </div>

    <!-- youtube link -->
    <div v-if="recipe_data.youtubeLink">
      <a :href="recipe_data.youtubeLink" target="_blank">Youtube Full 영상보기</a>
    </div>

    <!-- 이용후기 -->
    <!-- 구현이 된다면.... -->

    <!-- 밀어서 공개하기 -->
    <div v-if="recipe_data.open === false && recipe_data.writer.id === this.id">
      {{ $t("description.publishRecipe") }}<br />
      <button @click="updateOpen">{{ $t("button.slideAndPublish") }}</button>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  name: "RecipeDetailView",
  data: () => ({
    recipeId: 0,
    recipe_data: [],
  }),

  computed: {
    ...mapGetters("user", ["id"]),
  },

  mounted() {
    this.recipeId = this.$route.params.recipeId;
    this.getRecipeById(this.recipeId);
  },

  methods: {
    async getRecipeById(id) {
      const response = await this.$api(
        `${this.$API_SERVER}/Recipe/detail=${id}`,
        "get"
      ).then((res) => {
        if (res.status === this.HTTP_OK) {
          this.recipe_data = res.data;
        }
      });
    },
    async updateOpen() {
      await this.$api(
        `${this.$API_SERVER}/api/Recipe/updateOpen=${this.recipeId}`,
        "get"
      ).then((res) => {
        if (res.status === this.HTTP_OK) {
          console.log("레시피 공개 완료");
        }
      });
    },
  },
};
</script>

<style></style>
