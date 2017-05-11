<template>
    <div class="typo">

        <div class="typo__content">
            <div :style="wrapperStyle">
                <div v-html="renderedContent"></div>
            </div>
        </div>

        <div class="typo__controls">
            <div>
                Global spacer: <b>{{ spacer }}px</b><br><input type="range" v-model="spacer">
            </div>
            <div>
                Box padding:<br><input class="typo__number" type="number" v-model="paddingRatio" size="2"> × {{ spacer }} = {{ paddingRatio * spacer }} 
            </div>
            <div>
                Heading top margin:<br><input class="typo__number" type="number" v-model="headerTopMarginRatio" size="2"> × {{ spacer }} = {{ this.spacer * this.headerTopMarginRatio }} 
            </div>
            <div>
                Heading bottom margin:<br><input class="typo__number" type="number" v-model="headerBottomMarginRatio" size="2"> × {{ spacer }} = {{ this.spacer * this.headerBottomMarginRatio }} 
            </div>
            <div>
                Paragraph bottom margin:<br><input class="typo__number" type="number" v-model="paragraphMarginRatio" size="2"> × {{ spacer }} = {{ this.spacer * this.paragraphMarginRatio }} 
            </div>
        </div>

    </div>

</template>

<script>

    import marked from 'marked'
    import inline from '@f/to-inline-style'

    var renderer = new marked.Renderer();

    export default {
        props: {
            value: { default: '' }
        },
        data: () => ({
            content: '',
            spacer: 12,
            paddingRatio: 0,
            headerTopMarginRatio: 0,
            headerBottomMarginRatio: 0,
            paragraphMarginRatio: 0
        }),
        computed: {
            wrapperStyle() {
                return {
                    width: '60vw',
                    background: 'white',
                    padding: this.spacer * this.paddingRatio + 'px',

                }
            },
            headingStyle() {
                return {
                    color: 'blue',
                    marginTop: this.spacer * this.headerTopMarginRatio,
                    marginBottom: this.spacer * this.headerBottomMarginRatio
                }
            },
            paragraphStyle() {
                return {
                    marginBottom: this.spacer * this.paragraphMarginRatio
                }
            },
            renderedContent() {
                renderer.heading = (text, level) => {
                    return `<h${level} style="${inline(this.headingStyle)}">${text}</h1>`
                }
                renderer.paragraph = text => {
                    return `<p style="${inline(this.paragraphStyle)}">${text}</p>`
                }
                return marked(this.content, {breaks: true, renderer})
            }
        },
        mounted() {

            this.$http.get(this.value).then(res => {
                this.content = res.data
            })
        }
    }

</script>

<style>
    .typo {
        width: 100vw;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .typo__content {
        width: 75vw;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        background: #eee;
    }
    .typo__controls {
        height: 100vh;
        width: 25vw;
        padding: 2em 5em 2em 2em;
        font-family: Rubik, sans-serif;
        font-size: 14px;
        color: gray;
        overflow: scroll;
        line-height: 1.5em;
    }
    .typo__controls > div {
        margin-bottom: 0.75em;
    }
    .typo__controls h3 {
        margin: 0.5em 0 0.25em 0;
        opacity: 0.95;
    }
    .typo__number {
        width: 50px;
        padding: 5px;
    }

</style>