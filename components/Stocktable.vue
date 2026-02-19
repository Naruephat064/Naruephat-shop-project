<template>
    <v-card>
        <v-card-title class="d-flex pt-8 px-8">
            <h3 class="font-weight-bold">รายการสั่งซื้อ</h3>
            <v-spacer></v-spacer>
            <v-text-field
                v-model="search"
                label="ค้นหา"
                variant="outlined"
                prepend-inner-icon="mdi-magnify"
            ></v-text-field>
        </v-card-title>

        <v-divider></v-divider>

        <v-data-table
            v-model:search="search"
            :headers="headers"
            :items="orders"
            hover
        >
            <template v-slot:item.actions="{ item }">
                <v-btn
                    color="error"
                    variant="text"
                    icon
                    @click="deleteOrder(item.id)"
                >
                    <i class="fas fa-trash"></i>
                </v-btn>
            </template>
        </v-data-table>
    </v-card>
</template>

<script setup>
import { ref, onMounted } from "vue"

let search = ref()
let orders = ref([])
let headers = ref([
    {
        title: 'หมายเลข',
        value: 'id',
        align: 'center',
        sortable: true
    },
    {
        title: 'สินค้า',
        value: 'product_name',
        sortable: true
    },
    {
        title: 'ราคา',
        value: 'price'
    },
    {
        title: 'ชื่อผู้ซื้อ',
        value: 'userFullname'
    },
    {
        title: 'วันที่สั่ง',
        value: 'order_date'
    },
    {
        title: 'Action',
        value: 'actions'
    }
])

onMounted(async ()=>{
    await getOrders()
})

async function getOrders() {
    let response = await $fetch('http://localhost/dino-api/orders.php', {
        method: 'GET',
        params: {
            fn: 'getAll'
        }
    })

    if(response.status == true) {
        orders.value = JSON.parse(JSON.stringify(response.data))
    }
}

async function deleteOrder(id) {
    if (!confirm("ต้องการลบหรือไม่?")) return

    let response = await $fetch('http://localhost/dino-api/orders.php?fn=delete', {
        method: 'POST',
        body: {
            id: id
        }
    })

    if(response.status == true) {
        await getOrders()
    } else {
        alert("ลบไม่สำเร็จ")
    }
}
</script>
