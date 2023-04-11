<template>
    <vue-basic-alert :duration="300" :closeIn="2000" ref="alert" />
    <section class="order-section">

        <div class="heading">
            <span>Đặt Bàn</span>
            <h3>Tận Hưởng Khoảnh Khắc Của Bạn</h3>
        </div>

        <div class="icons-container">

            <div class="icons ">
                <img src="../assets/images/icon-1.png" alt="">
                <h3>7:00am to 10:00pm</h3>
            </div>

            <div class="icons">
                <img src="../assets/images/icon-2.png" alt="">
                <h3>+84 975 695 547</h3>
            </div>

            <div class="icons">
                <img src="../assets/images/icon-3.png" alt="">
                <h3>HUTECH - Thu Duc Campus</h3>
            </div>

        </div>

        <!-- booking form -->
        <form id="bookTableForm" @submit="handleSubmit" novalidate autocomplete="off">

            <div class="row">
                <div class="input-box">
                    <label for="uName">Họ và Tên</label>
                    <input type="text" name="uName" id="uName" v-model="orderObj.name">
                    <p v-if="errorObj.nameErr.length > 0">{{ errorObj.nameErr[0] }}</p>
                </div>
                <div class="input-box">
                    <label for="uPhone">Số điện thoại</label>
                    <input type="text" name="uPhone" id="uPhone" v-model="orderObj.phone">
                    <p v-if="errorObj.phoneErr.length > 0">{{ errorObj.phoneErr[0] }}</p>
                </div>
            </div>

            <div class="row">
                <div class="input-box">
                    <label for="oPeople">Số Người</label>
                    <input type="number" name="oPeople" id="oPeople" v-model="orderObj.people">
                    <p v-if="errorObj.peopleErr.length > 0">{{ errorObj.peopleErr[0] }}</p>
                </div>
                <div class="input-box">
                    <label for="oTables">Số Bàn</label>
                    <input type="number" name="oTables" id="oTables" v-model="orderObj.tables">
                    <p v-if="errorObj.tablesErr.length > 0">{{ errorObj.tablesErr[0] }}</p>
                </div>
            </div>

            <div class="row">
                <div class="input-box">
                    <label for="uCard">Thẻ Thành Viên Của Bạn</label>
                    <input type="text" name="uCard" id="uCard" v-model="orderObj.card">
                    <p v-if="errorObj.cardErr.length > 0">{{ errorObj.cardErr[0] }}</p>
                </div>
                <div class="input-box">
                    <label for="oWhen">Thời Gian</label>
                    <input type="datetime-local" name="oWhen" id="oWhen" v-model="orderObj.when"
                        @click="availableTime()">
                    <p v-if="errorObj.whenErr.length > 0">{{ errorObj.whenErr[0] }}</p>
                </div>
            </div>

            <div class="row">
                <div class="input-box">
                    <label for="uMessage">Ghi Chú</label>
                    <textarea placeholder="Tin nhắn của bạn, Bạn có muốn trang trí bàn của bạn" name="uMessage"
                        id="uMessage" cols="30" rows="10" v-model="orderObj.note"></textarea>
                </div>
                <div class="input-box">
                    <iframe class="map"
                    src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3918.4206639905988!2d106.78291401458974!3d10.855574792267841!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3175276e7ea103df%3A0xb6cf10bb7d719327!2zSHV0ZWNoIEtodSBFIC0gVHJ1bmcgVMOibSDEkMOgbyBU4bqhbyBOaMOibiBM4buxYyBDaOG6pXQgTMaw4bujbmcgQ2Fv!5e0!3m2!1svi!2s!4v1672736035162!5m2!1svi!2s"
                        loading="lazy"></iframe>
                </div>
            </div>

            <input type="submit" value="Book Now" class="btn">
        </form>

    </section>
</template>

<script>
import axios from 'axios';
import VueBasicAlert from 'vue-basic-alert'
export default {
    name: "Table",

    data() {
        return {
            orderObj: { name: "", phone: "", people: "", tables: "", card: "", when: "", note: "" },
            errorObj: { nameErr: [], phoneErr: [], peopleErr: [], tablesErr: [], cardErr: [], whenErr: [] },
        }
    },

    methods: {
        availableTime: function () {
            var now = new Date();
            var day = ("0" + now.getDate()).slice(-2);
            var currentMonth = ("0" + (now.getMonth() + 1)).slice(-2);
            // Thay doi thoi gian
            var maxMonth = ("0" + (now.getMonth() + 3)).slice(-2);
            var hour = ("0" + (now.getHours())).slice(-2);
            var min = ("0" + (now.getMinutes())).slice(-2);
            var minRange = now.getFullYear() + "-" + currentMonth + "-" + day + "T" + hour + ":" + min;
            var maxRange = now.getFullYear() + "-" + maxMonth + "-" + day + "T" + hour + ":" + min;

            document.getElementById("oWhen").setAttribute("min", minRange);
            document.getElementById("oWhen").setAttribute("max", maxRange);
        },

        resetCheckErr: function () {
            this.errorObj.nameErr = [];
            this.errorObj.phoneErr = [];
            this.errorObj.peopleErr = [];
            this.errorObj.tablesErr = [];
            this.errorObj.cardErr = [];
            this.errorObj.whenErr = [];
        },

        checkEmptyErr: function () {
            for (var typeErr in this.errorObj) {
                if (this.errorObj[typeErr].length != 0) {
                    return false;
                }
            }
            return true;
        },



        checkForm: function () {
            this.resetCheckErr();

            // Name validate
            if (!this.orderObj.name) {
                this.errorObj.nameErr.push("Vui lòng nhập tên");
            }
            else {
                if (!/^[A-Za-z]+$/.test(this.orderObj.name.replace(/\s/g, ""))) {
                    this.errorObj.nameErr.push('Tên chỉ chứa chữ');
                }
            }

            // Phone validate
            if (!this.orderObj.phone) {
                this.errorObj.phoneErr.push('Vui lòng nhập số điện thoại');
            }
            else {
                if (!this.orderObj.phone.startsWith('84')) {
                    this.errorObj.phoneErr.push('Số điện thoại bắt đầu từ 84');
                }

                if (!/[0-9]{10}/.test(this.orderObj.phone)) {
                    this.errorObj.phoneErr.push('Số điện thoại chỉ chứa số');
                }

                if (this.orderObj.phone.length != 11) {
                    this.errorObj.phoneErr.push('Số điện thoại gồm 11 chữ số');
                }
            }

            if (!this.orderObj.people) {
                this.errorObj.peopleErr.push("Vui lòng nhập số người");
            }
            else {
                if (parseInt(this.orderObj.people) > 100) {
                    this.errorObj.peopleErr.push("Mỗi cửa hàng chỉ có thể phục vụ 100 người cùng một lúc");
                }

                if (parseInt(this.orderObj.people) < 1) {
                    this.errorObj.peopleErr.push("Số người phải lớn hơn hoặc bằng 1");
                }
            }

            if (!this.orderObj.tables) {
                this.errorObj.tablesErr.push("Vui lòng nhập số bàn");
            }
            else {
                if (parseInt(this.orderObj.tables) > 50) {
                    this.errorObj.tablesErr.push("Mỗi cửa hàng chỉ được đặt tối đa 50 bàn");
                }

                if (parseInt(this.orderObj.tables) < 1) {
                    this.errorObj.tablesErr.push("Số bàn phải lớn hơn hoặc bằng 1");
                }

                if (parseInt(this.orderObj.people) < parseInt(this.orderObj.tables)) {
                    this.errorObj.tablesErr.push("Số bàn phải ít hơn số người");
                }
            }

            if (this.orderObj.card) {
                if (!/[0-9]{10}/.test(this.orderObj.card)) {
                    this.errorObj.cardErr.push('số thẻ phải là số');
                }

                if (this.orderObj.card.length != 10) {
                    this.errorObj.cardErr.push('số thẻ gồm 11 số');
                }
            }

            if (!this.orderObj.when) {
                this.errorObj.whenErr.push("Vui lòng nhập thời gian");
            }
            else {
                let minRange = document.getElementById("oWhen").getAttribute("min");
                let maxRange = document.getElementById("oWhen").getAttribute("max");
                let dateMin = new Date(minRange);
                let dateMax = new Date(maxRange);
                let dateInput = new Date(this.orderObj.when);

                if (dateInput === "Invalid Date") {
                    this.errorObj.whenErr.push("Nhập ngày không hợp lệ");
                }

                if (dateInput.getTime() < dateMin.getTime() || dateInput.getTime() > dateMax.getTime()) {
                    this.errorObj.whenErr.push("Phạm vi đặt trước có sẵn là từ nay đến hai tháng tới");
                }

                if (dateInput.getHours() < 7 || dateInput.getHours() > 22) {
                    this.errorObj.whenErr.push("Cửa hàng mở cửa từ 7:00 AM đến 10:00 PM hàng ngày");
                }
            }


        },

        async handleSubmit(e) {
            this.checkForm();

            if (!this.checkEmptyErr()) {
                e.preventDefault();
            } else {
                e.preventDefault();

                let data = {
                    book_name: this.orderObj.name,
                    book_phone: parseInt(this.orderObj.phone),
                    book_people: parseInt(this.orderObj.people),
                    book_tables: parseInt(this.orderObj.tables),
                    user_id: parseInt(this.orderObj.card),
                    book_when: this.orderObj.when,
                    book_note: this.orderObj.note,
                }

                await axios.post("/booking", data);

                this.$refs.alert.showAlert('Thành công', 'Cảm ơn bạn! Chúng tôi sẽ gọi lại cho bạn sớm để xác nhận đơn hàng', 'Đặt bàn thành công !')
                document.getElementById("bookTableForm").reset();
            }
        }
    },

    components: {
        VueBasicAlert
    }

}
</script>

<style scoped>
.order-section {
    padding: 2rem 9%;
}

.order-section .icons-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(40rem, 1fr));
    gap: 1.5rem;
    margin-bottom: 2rem;
}

.order-section .icons-container .icons {
    border-radius: .5rem;
    padding: 2rem;
    text-align: center;
    background: #f7f7f7;
}

.order-section .icons-container .icons img {
    height: 10rem;
}

.order-section .icons-container .icons h3 {
    font-size: 2rem;
    color: #130f40;
    margin-top: .5rem;
}

.order-section form {
    background: #f7f7f7;
    padding: 2rem;
    border-radius: .5rem;
}

.order-section form .row {
    justify-content: space-between;
}

.order-section form .row .input-box {
    width: 49%;
    padding: 1.8rem 0;
}

.order-section form .row label {
    font-size: 1.7rem;
    color: #666;
}

.order-section form .row p {
    font-size: 1.5rem;
    position: absolute;
    color: rgb(243, 47, 47);
    margin: 0;
    padding-top: 5px;
}

.order-section form .row input,
.order-section form .row textarea {
    width: 100%;
    margin-top: .5rem;
    padding: 1rem 1.2rem;
    width: 100%;
    border-radius: .5rem;
    font-size: 1.6rem;
    text-transform: none;
    color: #130f40;
}

.order-section form .row textarea {
    height: 20rem;
    resize: none;
}

.order-section form .row .map {
    height: 100%;
    width: 100%;
    border-radius: .5rem;
}

@media (max-width: 768px) {
    .order form .row .input-box {
        width: 100%;
    }

    .order-section form .row {
        display: block;
        max-width: 100%;
        width: 100%;
        margin: 0;
    }

    .order-section form .row .input-box {
        width: 100%;
    }

}

@media (max-width: 576px) {
    .order-section .icons-container {
        grid-template-columns: repeat(auto-fit, minmax(30rem, 1fr));
    }
}
</style>