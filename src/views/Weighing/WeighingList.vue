<template>
    <aside class="weight is-fullheight">
        <div class="columns">
            <p class="column is-5 is-size-4 is-flex pt-5 pl-5">Eventos
            <p class="has-text-weight-bold">&nbsp;> Pesagem</p>
            </p>

        </div>

        <div class="table-div">
            <table class="table">
                <thead class="header-table">
                    <tr>
                        <th>Dt.</th>
                        <th>Status</th>
                        <th>Data</th>
                        <th>Brinco</th>
                        <th>Peso</th>
                        <th></th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in weighingList" :key="item.id">
                        <th>
                            <button @click="onClickDetailPage(item.id)" class="button btn-detail">!</button>
                        </th>

                        <th>
                            <span v-if="!item.inactive" class="tag is-success"></span>
                            <span v-if="item.inactive" class="tag is-danger"></span>
                        </th>

                        <th>{{ item.date }}</th>

                        <th>{{ item.cattle.earring }}</th>

                        <th>{{ item.weight }}</th>

                        <th>
                            <button @click="onClickDetailPage(item.id)" class="button btn-edit">E</button>
                        </th>

                        <th>
                            <button @click="onClickDetailPage(item.id)" class="button btn-delet">X</button>
                        </th>
                    </tr>
                </tbody>
            </table>
        </div>
    </aside>
</template>
  
<script lang="ts">
import { Vue } from "vue-class-component";
import { PageRequest } from "@/model/page/page-request";
import { PageResponse } from "@/model/page/page-response";
import { WeighingClient } from "@/client/weighing.client";
import { Weighing } from "@/model/weighing.model";

export default class WeighingList extends Vue {
    private pageRequest: PageRequest = new PageRequest();
    private pageResponse: PageResponse<Weighing> = new PageResponse();
    public weighingList: Weighing[] = [];
    private weighingClient!: WeighingClient;

    public mounted(): void {
        this.weighingClient = new WeighingClient();
        this.listAll();
    }

    public listAll(): void {
        this.weighingClient.findByAll(this.pageRequest).then(
            (success) => {
                this.pageResponse = success;
                this.weighingList = this.pageResponse.content;
            },
            (error) => console.log(error)
        );
    }

    public onClickDetailPage(idWeighing: number) {
        this.$router.push({ name: 'weighingDetail', params: { id: idWeighing, model: 'detail' } })
    }
}
</script>
  
<style scoped>
.weight {
    width: 100%;
}

.table-div {
    padding: 30px;
}

.table {
    width: 100%;
}

.tag {
    border-radius: 50px;
    padding: 12px;
}

.btn-detail {
    font-size: 13px;
    background-color: #ffffff;
    border-color: #0093ff;
    border-width: 3px;
    border-radius: 150px;
    color: #0093ff;
    font-weight: bold;
    padding: 12px;
}

.btn-detail:hover {
    background-color: #ffffff;
    transform: translate(-1px, -1px);
    transition: 1s;
    border-color: #00c1ff;
    color: #00c1ff;
    font-weight: bold;
    box-shadow: 2px 5px 10px #a7a7a7;
}

.btn-edit {
    font-size: 13px;
    background-color: #ffffff;
    border-color: #1ba500;
    border-width: 3px;
    border-radius: 150px;
    color: #1ba500;
    font-weight: bold;
    padding: 12px;
}

.btn-edit:hover {
    background-color: #ffffff;
    transform: translate(-1px, -1px);
    transition: 1s;
    border-color: #25e000;
    color: #25e000;
    font-weight: bold;
    box-shadow: 2px 5px 10px #a7a7a7;
}

.btn-delet {
    font-size: 13px;
    background-color: #ffffff;
    border-color: #AB0303;
    border-width: 3px;
    border-radius: 150px;
    color: #AB0303;
    font-weight: bold;
    padding: 12px;
}

.btn-delet:hover {
    background-color: #ffffff;
    transform: translate(-1px, -1px);
    transition: 1s;
    border-color: #dd0000;
    color: #dd0000;
    font-weight: bold;
    box-shadow: 2px 5px 10px #a7a7a7;
}
</style>