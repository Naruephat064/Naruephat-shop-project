<template>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>สินค้า</th>
        <th>ราคา</th>
        <th>จัดการ</th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="item in orders" :key="item.id">
        <td>{{ item.id }}</td>
        <td>{{ item.product_name }}</td>
        <td>{{ item.price }}</td>
        <td>
          <button @click="deleteOrder(item.id)" class="delete-btn">
            ลบ
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const orders = ref([])

const fetchOrders = async () => {
  const res = await fetch('http://localhost/dino-api/get_orders.php')
  orders.value = await res.json()
}

const deleteOrder = async (id) => {
  if (!confirm('ต้องการลบหรือไม่?')) return

  await fetch('http://localhost/dino-api/delete_candidate.php', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ id })
  })

  fetchOrders()
}

onMounted(fetchOrders)
</script>

<style scoped>
.delete-btn {
  background: red;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 6px;
  cursor: pointer;
}
</style>
