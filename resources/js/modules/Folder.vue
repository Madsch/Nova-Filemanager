<template>
    <transition name="fade">
        <template v-if="view == 'grid'">
            <div @click="goToFolder"
                 ref="card"
                 :loading="loading"
                 :class="{ 'opacity-50': dragOver }"
                 class="card relative flex flex-wrap justify-center border border-lg border-50 overflow-hidden px-0 py-0 cursor-pointer">

                <template v-if="loading">
                    <div class="rounded-lg flex items-center justify-center absolute pin z-50">
                        <loader class="text-60" />
                    </div>
                </template>

                <template v-if="file.id == 'folder_back'">
                    <svg  class="w-2/3 h-5/6" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                        <path d="M20 6a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6c0-1.1.9-2 2-2h7.41l2 2H20zM4 6v12h16V8h-7.41l-2-2H4z" fill="#B3C1D1"/>
                        <path fill="#b3c1d1" d="M12.307 10.628v5a.32.32 0 0 1-.64 0v-5l-1.68 1.71a.323.323 0 0 1-.49-.42l2.25-2.25a.32.32 0 0 1 .45 0l2.25 2.25a.323.323 0 1 1-.42.49l-1.71-1.71-.01-.07z"/>
                    </svg>

                    <div class="h-1/6 w-full text-center text-xs  border-t border-30 bg-50 flex items-center justify-center">
                        {{ __('Go up') }}
                    </div>
                </template>

                <template v-else>

                    <svg class="w-2/3 h-5/6" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path fill="#B3C1D1"  d="M20 6a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6c0-1.1.9-2 2-2h7.41l2 2H20zM4 6v12h16V8h-7.41l-2-2H4z"/></svg>

                    <div class="actions-grid absolute pin-t pin-r pr-2 pt-2">
                        <div class="flex flex-wrap text-70">
                            <div class="cursor-pointer" @click.prevent="deleteFolder($event)">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20" aria-labelledby="delete" class="fill-current"><path fill-rule="nonzero" d="M6 4V2a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2h5a1 1 0 0 1 0 2h-1v12a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6H1a1 1 0 1 1 0-2h5zM4 6v12h12V6H4zm8-2V2H8v2h4zM8 8a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0V9a1 1 0 0 1 1-1zm4 0a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0V9a1 1 0 0 1 1-1z"></path></svg>
                            </div>
                            <div class="cursor-pointer ml-2" @click.prevent="editFolder($event)">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20" aria-labelledby="edit" class="fill-current"><path d="M4.3 10.3l10-10a1 1 0 0 1 1.4 0l4 4a1 1 0 0 1 0 1.4l-10 10a1 1 0 0 1-.7.3H5a1 1 0 0 1-1-1v-4a1 1 0 0 1 .3-.7zM6 14h2.59l9-9L15 2.41l-9 9V14zm10-2a1 1 0 0 1 2 0v6a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V4c0-1.1.9-2 2-2h6a1 1 0 1 1 0 2H2v14h14v-6z"></path></svg>
                            </div>
                        </div>
                    </div>

                    <div class="h-1/6 w-full text-center text-xs  border-t border-30 bg-50 flex items-center justify-center">
                        {{ file.name }}
                    </div>

                </template>

                

                

            </div>
        </template>

        <template v-if="view == 'list'">

            <template v-if="file.id == 'folder_back'">
                <tr @click="goToFolder" :loading="loading" v-bind:key="file.id"  class="cursor-pointer">
                    <td>
                        <div class="w-full flex justify-center items-center">
                            <template v-if="loading">
                                <div class="rounded-lg flex items-center justify-center absolute pin z-50">
                                    <loader class="text-60" />
                                </div>
                            </template>

                            <svg class="w-24 h-24" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                                <path d="M20 6a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6c0-1.1.9-2 2-2h7.41l2 2H20zM4 6v12h16V8h-7.41l-2-2H4z" fill="#B3C1D1"/>
                                <path fill="#b3c1d1" d="M12.307 10.628v5a.32.32 0 0 1-.64 0v-5l-1.68 1.71a.323.323 0 0 1-.49-.42l2.25-2.25a.32.32 0 0 1 .45 0l2.25 2.25a.323.323 0 1 1-.42.49l-1.71-1.71-.01-.07z"/>
                            </svg>
                        </div>
                    </td>
                    <td>{{ __('Go up') }}</td>
                    <td></td>
                    <td></td>
                    <td></td>
                </tr>
            </template>
            <template v-else>
                <tr @click="goToFolder" :loading="loading" v-bind:key="file.id"  class="cursor-pointer">
                    <td>
                        <div class="w-full flex justify-center items-center">
                            <template v-if="loading">
                                <div class="rounded-lg flex items-center justify-center absolute pin z-50">
                                    <loader class="text-60" />
                                </div>
                            </template>

                            <svg class="w-24 h-24" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path fill="#B3C1D1"  d="M20 6a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6c0-1.1.9-2 2-2h7.41l2 2H20zM4 6v12h16V8h-7.41l-2-2H4z"/></svg>
                        </div>
                    </td>

                    <td>
                        {{ file.name }}
                    </td>

                    <td>
                        <template v-if="file.size">{{ file.size_human }}</template>
                    </td>

                    <td>
                        {{ file.date }}
                    </td>
                    <td>
                        <div class="flex flex-wrap text-70">
                            <div class="cursor-pointer" @click.prevent="deleteFolder($event)">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20" aria-labelledby="delete" class="fill-current"><path fill-rule="nonzero" d="M6 4V2a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2h5a1 1 0 0 1 0 2h-1v12a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6H1a1 1 0 1 1 0-2h5zM4 6v12h12V6H4zm8-2V2H8v2h4zM8 8a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0V9a1 1 0 0 1 1-1zm4 0a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0V9a1 1 0 0 1 1-1z"></path></svg>
                            </div>
                            <div class="cursor-pointer ml-2" @click.prevent="editFolder($event)">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20" aria-labelledby="edit" class="fill-current"><path d="M4.3 10.3l10-10a1 1 0 0 1 1.4 0l4 4a1 1 0 0 1 0 1.4l-10 10a1 1 0 0 1-.7.3H5a1 1 0 0 1-1-1v-4a1 1 0 0 1 .3-.7zM6 14h2.59l9-9L15 2.41l-9 9V14zm10-2a1 1 0 0 1 2 0v6a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V4c0-1.1.9-2 2-2h6a1 1 0 1 1 0 2H2v14h14v-6z"></path></svg>
                            </div>
                        </div>
                    </td>
                </tr>
            </template>

            
        </template>
    </transition>
</template>

<script>
export default {
    props: {
        file: {
            type: Object,
            default: function() {
                return { name: '' };
            },
            required: true,
        },
        view: {
            type: String,
            default: 'grid',
            required: false,
        },
    },

    data: () => ({
        loading: true,
        missing: false,
        dragOver: false,
    }),

    mounted() {
        this.loading = false;
    },

    methods: {
        goToFolder() {
            this.$emit('goToFolderEvent', this.file.path);
        },

        deleteFolder(e) {
            this.stopDefaultActions(e);
            this.$emit('delete', 'folder', this.file.path);
        },

        editFolder(e) {
            this.stopDefaultActions(e);
            this.$emit('rename', 'folder', this.file.path);
        },

        stopDefaultActions(e) {
            e.preventDefault();
            e.stopPropagation();
        },
    },
};
</script>

<style lang="scss" scoped>
.card {
    padding: 0 !important;

    &:hover {
        > svg {
            opacity: 0.5;
        }

        > .actions-grid {
            display: flex;
        }
    }
}

.h-5\/6 {
    height: 83.33333%;
}

.h-1\/6 {
    height: 16.66667%;
}

.h-2\/3 {
    height: 75%;
}

.actions-grid {
    display: none;
}

.w-24 {
    width: 24px;
}

.h-24 {
    height: 24px;
}
</style>
