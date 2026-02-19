<template>
  <div class="admin-orders-page">
    <h2>รายการสั่งซื้อทั้งหมด</h2>

    <table class="orders-table">
      <thead>
        <tr>
          <th>ID</th>
          <th>สินค้า</th>
          <th>ราคา</th>
          <th>ชื่อผู้ซื้อ</th>
          <th>วันที่สั่ง</th>
          <th>จัดการ</th> <!-- ✅ เพิ่ม -->
        </tr>
      </thead>

      <tbody>
        <tr v-for="order in orders" :key="order.id">
          <td>{{ order.id }}</td>
          <td>{{ order.product_name }}</td>
          <td>{{ order.price }}</td>
          <td>{{ order.userFullname }}</td>
          <td>{{ order.order_date }}</td>
          <td>
          <button type="button" @click="deleteOrder(order.id)">
            ลบ
          </button>


          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script setup>
import { ref, onMounted } from 'vue'

definePageMeta({
  layout: "admin"
})

const orders = ref([])

const fetchOrders = async () => {
  try {
    const res = await fetch('http://localhost/dino-api/get_orders.php')
    orders.value = await res.json()
  } catch (err) {
    console.error("โหลดข้อมูลไม่สำเร็จ", err)
  }
}

const deleteOrder = async (id) => {
  if (!confirm("ต้องการลบหรือไม่?")) return

  const res = await fetch('http://localhost/dino-api/delete_order.php', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ id })
  })

  const result = await res.json()
  console.log(result)

  fetchOrders()
}

const testClick = () => {
  console.log("ปุ่มทำงานแล้ว")
}


onMounted(fetchOrders)
</script>


<style>
.admin-orders-page {
  padding: 24px;
}

.orders-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 12px;
}

.orders-table th,
.orders-table td {
  border: 1px solid #eee;
  padding: 10px;
  text-align: center;
}

.orders-table th {
  background: #f5f5f5;
}

.delete-btn {
  background: red;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 6px;
  cursor: pointer;
}

.delete-btn:hover {
  background: darkred;
}

</style>
