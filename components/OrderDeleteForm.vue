<template>
  <div>
    <v-card class="mx-auto my-8" elevation="5">
      <v-card-item class="bg-red-darken-2">
        <v-card-title class="font-weight-bold pa-3">
          <v-icon class="fas fa-box mr-2"></v-icon>
          ลบรายการสั่งซื้อ
        </v-card-title>
      </v-card-item>

      <v-card-text class="pa-8">
        <v-form ref="form" @submit.prevent="destroy">
          <v-row>
            <v-col cols="12" md="3">
              <v-text-field
                v-model="order.id"
                label="หมายเลข"
                variant="outlined"
                readonly
              />
            </v-col>

            <v-col cols="12" md="9">
              <v-text-field
                v-model="order.product_name"
                label="ชื่อสินค้า"
                variant="outlined"
                readonly
              />
            </v-col>

            <v-col cols="12">
              <v-text-field
                v-model="confirmDelete"
                label="พิมพ์ 'ลบ' หรือ 'delete' เพื่อยืนยัน"
                variant="outlined"
              />
            </v-col>

            <v-col cols="12" class="text-center">
              <v-btn
                type="submit"
                color="error"
                :disabled="confirmDelete!='ลบ' && confirmDelete.toLowerCase()!='delete'"
              >
                ลบ
              </v-btn>
            </v-col>
          </v-row>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>

<script setup>
import Swal from 'sweetalert2'
import { ref, onMounted } from 'vue'

const props = defineProps({
  orderID: {
    type: Number,
    default: 0
  }
})

const emit = defineEmits(['deleteStatus'])

const form = ref(null)
let order = ref({})
let confirmDelete = ref('')

onMounted(async () => {
  form.value?.resetValidation()
  await getOrder()
})

async function getOrder() {
  let result = await $fetch(
    'http://localhost/dino-api/orders.php?fn=getByID',
    {
      method: 'GET',
      params: {
        id: props.orderID
      }
    }
  )

  if (result.status) {
    order.value = result.data
  }
}

async function destroy() {
  const { valid } = await form.value.validate()

  if (valid) {
    let res = await $fetch(
      'http://localhost/dino-api/orders.php?fn=delete',
      {
        method: 'POST',
        body: {
          id: props.orderID
        }
      }
    )

    if (res.status) {
      Swal.fire({
        title: "สำเร็จ",
        text: "ลบข้อมูลเรียบร้อย",
        icon: "success"
      })

      emit('deleteStatus', {
        status: true
      })

    } else {
      Swal.fire({
        title: "ผิดพลาด",
        text: "ลบข้อมูลไม่สำเร็จ",
        icon: "error"
      })
    }
  }
}
</script>
