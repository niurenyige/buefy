<template>
    <transition
        :enter-active-class="transition.enter"
        :leave-active-class="transition.leave">

        <div class="snackbar" v-show="isActive">
            <p class="text">{{ message }}</p>
            <div v-if="actionText" class="action" @click="action" :class="type">
                <button class="button is-dark">{{ actionText }}</button>
            </div>
        </div>
    </transition>
</template>

<script>
    import config from '../../utils/config'
    import NoticeMixin from '../../utils/NoticeMixin.js'

    export default {
        data() {
            return {
                newDuration: this.duration || config.defaultSnackbarDuration
            }
        },
        mixins: [NoticeMixin],
        props: {
            actionText: {
                type: String,
                default: 'OK'
            },
            duration: {
                type: Number
            },
            onAction: {
                type: Function,
                default: () => {}
            }
        },
        methods: {
            /**
             * Add component to the DOM with it's classes,
             * called from the Mixin.
             */
            insertEl() {
                this.parent.className = ''
                this.parent.classList.add('notices', this.position)
                this.parent.appendChild(this.$el)
            },

            /**
             * Click listener.
             * Call action prop before closing (from Mixin).
             */
            action() {
                this.onAction()
                this.close()
            }
        }
    }
</script>
