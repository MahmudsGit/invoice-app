<template>
    <div class="container">
        <div class="invoices">
            <div class="card__header">
                <div>
                    <h2 class="invoice__title">Invoice</h2>
                </div>
                <div>

                </div>
            </div>
            <div>
                <div class="card__header--title ">
                    <h1 class="mr-2">#{{ form.id }}</h1>
                    <p>{{ form.created_at }}</p>
                </div>
                <div>
                    <ul class="card__header-list">
                        <li>
                            <!-- Select Btn Option -->
                            <button class="selectBtnFlat" @click="print()">
                                <i class="fas fa-print"></i>
                                Print
                            </button>
                            <!-- End Select Btn Option -->
                        </li>
                        <li>
                            <!-- Select Btn Option -->
                            <button class="selectBtnFlat" @click="onEdit(form.id)">
                                <i class=" fas fa-reply"></i>
                                Edit
                            </button>
                            <!-- End Select Btn Option -->
                        </li>
                        <li>
                            <!-- Select Btn Option -->
                            <button class="selectBtnFlat " @click="onDelete(form.id)">
                                <i class=" fas fa-pencil-alt"></i>
                                Delete
                            </button>
                            <!-- End Select Btn Option -->
                        </li>
                    </ul>
                </div>
            </div>
            <div class="table invoice">
                <div class="logo">
                    <img src="" alt="" style="width: 200px;">
                </div>
                <div class="invoice__header--title">
                    <p></p>
                    <p class="invoice__header--title-1">Invoice</p>
                    <p></p>
                </div>
                <div class="invoice__header--item">
                    <div>
                        <h2>Invoice To:</h2>
                        <p>Name: {{ form.customer ? form.customer.firstname : '' }}</p>
                        <p>Email: {{ form.customer ? form.customer.email : '' }}</p>
                        <p>Address: {{ form.customer ? form.customer.address : '' }}</p>
                    </div>
                    <div>
                        <div class="invoice__header--item1">
                            <p>Invoice#</p>
                            <span>#{{ form.number }}</span>
                        </div>
                        <div class="invoice__header--item2">
                            <p>Date</p>
                            <span>{{ form.date }}</span>
                        </div>
                        <div class="invoice__header--item2">
                            <p>Due Date</p>
                            <span>{{ form.due_date }}</span>
                        </div>
                        <div class="invoice__header--item2">
                            <p>Reference</p>
                            <span>{{ form.reference }}</span>
                        </div>

                    </div>
                </div>
                <div class="table py1">
                    <div class="table--heading3">
                        <p>#</p>
                        <p>Item Description</p>
                        <p>Unit Price</p>
                        <p>Qty</p>
                        <p>Total</p>
                    </div>
                    <!-- item 1 -->

                    <div class="table--items3" v-for="(item, i) in form.invoice_items">
                        <p>{{ i + 1 }}</p>
                        <p>{{ item.product.description }}</p>
                        <p>$ {{ item.product.unit_price }}</p>
                        <p>{{ item.quantity }}</p>
                        <p>$ {{ item.quantity * item.product.unit_price }}</p>
                    </div>
                </div>
                <div class="invoice__subtotal">
                    <div>
                        <h2>Thank you for your business</h2>
                    </div>
                    <div>
                        <div class="invoice__subtotal--item1">
                            <p>Sub Total</p>
                            <span> $ {{ form.sub_total }}</span>
                        </div>
                        <div class="invoice__subtotal--item2">
                            <p>Discount</p>
                            <span>$ {{ form.discount }}</span>
                        </div>
                    </div>
                </div>
                <div class="invoice__total">
                    <div>
                        <h2>Terms and Conditions</h2>
                        <p>{{ form.terms_and_conditions }}</p>
                    </div>
                    <div>
                        <div class="grand__total">
                            <div class="grand__total--items">
                                <p>Grand Total</p>
                                <span>$ {{ form.total }}</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <br>
    </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import axios from "axios"
import { useRouter } from 'vue-router';

const router = useRouter()

onMounted(async () => {
    getInvoice()
})

let form = ref({ id: '' })

const props = defineProps({
    id: {
        type: String,
        default: ''
    }
})

const getInvoice = async () => {
    let response = await axios.get(`/api/show_invoice/${props.id}`)
    form.value = response.data.invoice
}

const print = async (id) => {
    window.print()
    router.push('/').catch(() => { })
}

const onEdit = async (id) => {
    router.push('/invoice/edit/' + id)
}

const onDelete = async (id) => {
    axios.get('/api/invoice_delete/' + id)
    router.push('/');
}
</script>