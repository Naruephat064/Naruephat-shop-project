<template>
  <div class="container">
    <h1 class="title">Our Products</h1>

    <!-- 🔹 สินค้า 1 -->
    <div class="product-card">
      <img
        src="https://sandovallegacygroup.com/wp-content/uploads/2025/08/Screenshot-2025-08-14-at-9.19.56-AM-854x675.png"
        alt="เสื้อยืด Dino Shop"
        class="product-image"
        loading="lazy"
      />

      <div class="product-info">
        <h2>เสื้อยืด โคตรเป</h2>
        <p class="description">
          เสื้อยืดผ้าดี ใส่สบาย ผู้ใส่เป็นนักการเงินระดับโลก และมีเกาะส่วนตัวเป็นของตัวเอง สินค้าราคามือสอง
        </p>
        <p class="price">ราคา 2,500 บาท</p>

        <button class="buy-btn" @click="order('เสื้อยืด Dino Shop', 2500)">
          get order!
        </button>
      </div>
    </div>

    <!-- 🔹 สินค้า 2 -->
    <div class="product-card">
      <img
        src="https://img.webike-cdn.net/374x280/moto_img/cg/12/11448/L_bk3pjvq0twdf7mo9g5erui1lnx.jpg"
        alt="แก้วน้ำ Dino"
        class="product-image"
        loading="lazy"
      />

      <div class="product-info">
        <h2>Wave ทรงเชง</h2>
        <p class="description">
          รถมือสองสภาพดี เลขไมล์1 เสียตั้งแต่ออกศูนย์ ของแต่งเพียบ
        </p>
        <p class="price">ราคา 99000 บาท</p>

        <button class="buy-btn" @click="order('Wave ทรงเชง', 99000)">
          get order!
        </button>
      </div>
    </div>

    <!-- 🔹 สินค้า 3 -->
    <div class="product-card">
      <img
        src="https://ichef.bbci.co.uk/ace/ws/800/cpsprodpb/c5a3/live/b68276a0-76bb-11ef-850e-8752ff026834.jpg.webp"
        alt="หมวก Dino"
        class="product-image"
        loading="lazy"
      />

      <div class="product-info">
        <h2>เรือดำน้ำ</h2>
        <p class="description">
          เรือดำน้ำมือสอง ผ่านการระเบิดมาแล้ว 1ครั้ง ตอนนี้นำกลับมาซ่อมใหม่ ยังไม่ได้ทดลองขับ
        </p>
        <p class="price">ราคา 189000 บาท</p>

        <button class="buy-btn" @click="order('เรือดำน้ำ', 189000)">
          get order!
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const order = async (productName, price) => {
  try {
    const response = await fetch('http://localhost/dino-api/insert_order.php', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
      },
      body: new URLSearchParams({
        product_name: productName,
        price: price,
        user_id: 1
      })
    })

    const result = await response.text()
    alert(result)

  } catch (error) {
    console.error(error)
    alert('เกิดข้อผิดพลาด')
  }
}
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: 40px auto;
  padding: 20px;
}

.title {
  font-size: 32px;
  margin-bottom: 30px;
}

.product-card {
  display: flex;
  gap: 30px;
  border: 1px solid #eee;
  border-radius: 12px;
  padding: 20px;
  margin-bottom: 25px;
  background: #f5f5f5;
}

.product-image {
  width: 300px;
  height: 200px;
  object-fit: cover;
  border-radius: 10px;
  background: #ddd;
}

.product-info {
  flex: 1;
}

.description {
  margin: 15px 0;
  color: #555;
}

.price {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 20px;
}

.buy-btn {
  background: black;
  color: white;
  padding: 12px 28px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
}

.buy-btn:hover {
  opacity: 0.85;
}

@media (max-width: 768px) {
  .product-card {
    flex-direction: column;
  }

  .product-image {
    width: 100%;
    height: 220px;
  }
}
</style>
