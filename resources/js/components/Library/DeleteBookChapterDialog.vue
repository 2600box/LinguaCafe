<template>
    <v-dialog v-model="value" persistent max-width="500px" height="300px">
        <v-card id="delete-book-chapter-dialog" class="rounded-lg">
            <v-progress-linear
                class="delete-dialog-delay"
                :value="deletionEnabledDelay"
                color="error"
                height="8"
            ></v-progress-linear>
            <v-card-title>
                <v-icon large class="mr-2" color="error">mdi-alert-circle</v-icon>
                <span class="text-h5">Delete chapter</span>
                <v-spacer></v-spacer>
                <v-btn icon @click="close">
                    <v-icon>mdi-close</v-icon>
                </v-btn>
            </v-card-title>
            <v-card-text class="pt-4 pb-6">
                Do you want to delete this chapter called {{ $props.chapterName }}?
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn rounded text @click="close">Cancel</v-btn>
                <v-btn rounded :dark="deletionEnabledDelay > 100" color="error" @click="confirm" :disabled="deletionEnabledDelay < 101">
                    <v-icon class="mr-2" color="white">mdi-delete</v-icon>
                    Delete
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
    export default {
        props: {
            value : Boolean,
            chapterId: Number,
            chapterName: String,
        },
        emits: ['input'],
        data: function() {
            return {
                deletionEnabledDelay: 0,
            };
        },
        mounted: function() {
            this.progressDelay();
        },
        methods: {
            progressDelay() {
                this.deletionEnabledDelay ++;

                if (this.deletionEnabledDelay < 101) {
                    setTimeout(this.progressDelay, 20);
                }
            },
            confirm() {
                this.$emit('confirm', this.$props.chapterId);
                this.$emit('input', false);
            },
            close() {
                this.$emit('input', false);
            }
        }
    }
</script>
