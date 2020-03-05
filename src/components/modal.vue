<template>
    <div>
        <ion-header>
            <ion-toolbar>
                <ion-title v-if="modify">Modify Your Book</ion-title>
                <ion-title v-else>Add a New Book</ion-title>
                <ion-button slot="end" v-on:click="dismissModal(null)">Close</ion-button>
            </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
            <form v-on:submit.prevent="addBooks">
                <ion-list lines="full">
                    <ion-items  position="stacked">
                        <ion-label>
                            Title:
                        </ion-label>
                        <ion-input @input="book.title = $event.target.value" :value="book.title" autocomplete="on" name="title" class="bottom-line" required type="text">
                        </ion-input>
                    </ion-items>
                    <ion-items position="stacked">
                        <ion-label>
                            Author:
                        </ion-label>
                        <ion-input @input="book.author = $event.target.value" :value="book.author" name="author" autocomplete="on" class="bottom-line" required type="text">
                        </ion-input>
                    </ion-items>
                    <ion-items position="stacked">
                        <ion-label position="floating">
                            Description:
                        </ion-label>
                        <ion-textarea @input="book.shortDescription = $event.target.value" :value="book.shortDescription"  name="description" autocomplete="on" class="bottom-line" required type="text">
                        </ion-textarea>
                    </ion-items>
                    <ion-items position="stacked">
                        <ion-label>
                            Image Url:
                        </ion-label>
                        <ion-input @input="book.thumbnailUrl = $event.target.value" :value="book.thumbnailUrl"  autocomplete="on" name="url" class="bottom-line" required type="url">
                        </ion-input>
                    </ion-items>
                    <ion-items position="stacked">
                        <ion-label>
                            Pages:
                        </ion-label>
                        <ion-input @input="book.pageCount = $event.target.value" :value="book.pageCount"  autocomplete="on" name="page" class="bottom-line" required type="number">
                        </ion-input>
                    </ion-items>
                    <ion-items>
                        <ion-button v-if="book.title === ''|| book.pageCount === '' || book.thumbnailUrl === '' || book.shortDescription === '' || book.author === ''" color="light" type="submit" class="bottom-line" expand="block">Add</ion-button>
                        <ion-button v-else-if="modify" color="primary" type="submit" class="bottom-line" expand="block">Modify</ion-button>
                        <ion-button v-else color="primary" type="submit" class="bottom-line" expand="block">Add</ion-button>
                    </ion-items>
                </ion-list>
            </form>
        </ion-content>
    </div>
</template>
<style scoped type="text/css">
    .bottom-line{
        border-bottom: solid 1px lightgray;
        margin-bottom: 10px;
    }
</style>
<script>
    export default {
        name: 'Modal',
        props:{
          modify:{type:Boolean, default: false}
        },
        data(){
            return{
                book:{
                    title: '',
                    isbn: null,
                    pageCount: '',
                    thumbnailUrl: '',
                    shortDescription: '',
                    author: ''
                }
            }
        },
        methods: {
            dismissModal: function (book) {
                return this.$ionic.modalController.dismiss(book)
            },
            addBooks: function () {
                this.book.isbn === null ? this.book.isbn = Math.floor(Math.random() * 10000000000) : false
                this.dismissModal(this.book)
            }
        }
    }
</script>