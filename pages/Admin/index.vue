<template>
  <div class="admin-content">
    <h1>รายการสั่งซื้อ</h1>

    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>สินค้า</th>
          <th>ราคา</th>
          <th>User ID</th>
          <th>วันที่สั่งซื้อ</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="order in orders" :key="order.id">
          <td>{{ order.id }}</td>
          <td>{{ order.product_name }}</td>
          <td>{{ order.price }}</td>
          <td>{{ order.user_id }}</td>
          <td>{{ order.order_date }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const orders = ref([])

onMounted(async () => {
  try {
    const res = await fetch('http://localhost/dino-api/get_orders.php')
    const data = await res.json()
    orders.value = data
  } catch (error) {
    console.error("โหลดข้อมูลไม่สำเร็จ:", error)
  }
})
</script>

<style scoped>
.admin-content {
  padding: 30px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  background: white;
}

th, td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
}

th {
  background-color: #f0f0f0;
}
</style>
