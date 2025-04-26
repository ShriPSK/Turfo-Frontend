<template>
    <div class="page-container">
        <div class="page-header">
            <div class="page-title">Turf List</div>
            <div class="search-field">
                <v-text-field v-model="search" hide-details variant="outlined" density="compact"
                    placeholder="Search by name or location" bg-color="#FFFFFF" prepend-inner-icon="mdi-magnify"
                    class="search-input"></v-text-field>
            </div>
        </div>
        <div class="table-container">
            <v-data-table :headers="headers" :items="filteredTurfList" :items-per-page="itemPerPage" hide-default-footer @page-count="pageCount = $event" :page.sync="page"
                fixed-header class="global-table">
                <template #item.amenities="{ item }">
                    <div>
                        {{ item.amenities.join(', ') }}
                    </div>
                </template>

                <template #item.status="{ item }">
                    <v-switch v-model="item.status" inset hide-details color="var(--bg-color)"></v-switch>
                </template>
                <template #item.pricePerHour="{ item }">
                    <div>
                        ₹ {{ item.pricePerHour }}
                    </div>
                </template>
            </v-data-table>

        </div>

        <div class="pagination-container">
            <div class="items-per-page-select">
                <v-select v-model="itemPerPage" :items="itemsPerPageOptions" item-title="text" item-value="value" hide-details variant="outlined" density="comfortable"
                    bg-color="#FFFFFF"></v-select>
            </div>
            <v-pagination v-model="page" v-if="itemPerPage != -1" :length="pageCount" :total-visible="7" bg-color="#FFFFFF" class="pagination"
                ></v-pagination>
        </div>
    </div>
</template>

<script>
import turfListJson from '../assets/json/turfList.json'
export default {
    data() {
        return {
            page: 1,
            itemPerPage: 10,
            pageCount: 10,
            turfList: turfListJson,
            headers: [
                {
                    title: 'Turf Name',
                    key: 'name',
                    class: 'table-header'
                },
                {
                    title: 'Location',
                    key: 'location',
                    class: 'table-header'
                },
                {
                    title: 'Price per Hour',
                    key: 'pricePerHour',
                    class: 'table-header'
                },
                {
                    title: 'Amenities',
                    key: 'amenities',
                    class: 'table-header'
                },
                {
                    title: 'Status',
                    key: 'status',
                    class: 'table-header'
                }
            ],
            search: null,
            itemsPerPageOptions: [
                {
                    text: '5', value: 5
                },
                {
                    text: '10', value: 10
                },
                {
                    text: '25', value: 25
                },
                {
                    text: 'All', value: -1
                }
            ],
            filteredTurfList: []
        }
    },
    computed: {
        paginatedFilteredList() {
            const start = (this.page - 1) * this.itemPerPage;
            const end = start + this.itemPerPage;
            return this.filteredTurfList.slice(start, end);
        }
    },

    watch: {
        search() {
            this.applySearch();
        },
        itemPerPage() {
            this.updatePageCount();
            this.page = 1;
        },
        filteredTurfList() {
            this.updatePageCount();
        }
    },
    methods: {
        applySearch() {
            if (!this.search) {
                this.filteredTurfList = this.turfList;
            } else {
                const searchLower = this.search.toLowerCase();
                this.filteredTurfList = this.turfList.filter(turf =>
                    turf.name.toLowerCase().includes(searchLower) ||
                    turf.location.toLowerCase().includes(searchLower)
                );
            }
            this.page = 1; // Reset to first page
        },
        updatePageCount() {
            this.pageCount = Math.ceil(this.filteredTurfList.length / this.itemPerPage);
        }
    },
    mounted() {
        this.applySearch();
    }
}
</script>

<style scoped>
.page-container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100vh;
    overflow: hidden;
    padding: 16px 32px;
    gap: 16px;
}

.page-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.page-title {
    font-weight: 600;
    font-size: 24px;
}

.search-field {
    width: 30%;
}

.table-container {
    height: 100%;
    overflow: hidden;
}

.global-table :deep(.v-data-table__th) {
    background: #535353 !important;
    color: white !important;
}

.pagination-container {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: 16px;
}

.items-per-page-select {
    width: 100px;
}

.pagination :deep( .v-pagination__item), .pagination :deep( .v-pagination__prev), .pagination :deep( .v-pagination__next){
    background: white !important;
    border-radius: 10px;
}

.pagination :deep( .v-pagination__prev) button:disabled {
    cursor: not-allowed !important;
}

.pagination :deep( .v-pagination__item--is-active){
    background: #535353 !important;
    color: white !important;
}
</style>
