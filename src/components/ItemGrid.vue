<script setup>
</script>

<template>
  <div class="card-body">
    <div class="item-grid">
      <div v-for="itemIdx in 16" :key="itemIdx" class="item">
        <img src="../assets/image/item.png" alt="Item" />
        <div class="item-label">
          <span class="item-text">AED x3</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.card-body {
  width: 100%;
  height: 460px;
  background: rgba(0, 0, 0, 0.4);
  border-radius: 4px;
  padding: 12px 8px;
  overflow-y: auto;

  &::-webkit-scrollbar {
    width: 4px;
  }

  &::-webkit-scrollbar-thumb {
    background: rgba(255, 255, 255, 0.2);
    border-radius: 10px;
  }

  .item-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;

    .item {
      display: flex;
      flex-direction: column;
      align-items: center;

      img {
       width: 100%;
       height: 100%;
        object-fit: contain;
      }

      .item-label {
        width: 100%;
        background: rgba(255, 255, 255, 0.8);
        text-align: center;

        .item-text {
          color: #000;
          font-size: 10px;
          font-weight: bold;
        }
      }
    }
  }
}
</style>
