<template>
   <div class="vue-simple-pagination">
    <nav v-if="moreThanOnePage">
        <ul :class="ulClass">
            <li :class="getClass('previous')">
                <a @click="triggerPageChange(currentPage-1)" aria-label="Previous">
                    <i class="fa fa-chevron-left" aria-hidden="true"></i>
                </a>
            </li>
            <li v-for="page in pages" :class="getClass(page.number)"><a v-if="page.number" @click="triggerPageChange(page.number)">
                {{ page.number }}</a><span v-if="pageNull(page)">...</span>
            </li>
            <li :class="getClass('next')">
                <a @click="triggerPageChange(currentPage+1)" aria-label="Next">
                    <i class="fa fa-chevron-right" aria-hidden="true"></i>
                </a>
            </li>
        </ul>
    </nav>
    </div>
</template>

<script>
    export default {
        props: {
            currentPage: {
                required: true,
                type: Number
            },
            pageCount: {
                required: true,
                type: Number
            },
            ulClass: {
                default: 'pagination'
            },
        },

        data: function () {
            return {
                pages: []
            };
        },

        watch: {
            currentPage(){
                this.buildPageList();
            }
        },

        computed: {
            moreThanOnePage() {
                // Whenever pageCount changes, it triggers the page list to be rebuilt
                this.buildPageList();
                return this.pageCount > 1;
            }
        },

        methods: {
            buildPageList() {
                this.pages = [];
                if (this.pageCount > 10) {
                    if (this.currentPage >= 7 &&  this.currentPage < this.pageCount - 5) {
                        this.makePagesRange(1, 2);
                        this.pages.push({
                            number: null
                        });
                        this.makePagesRange(this.currentPage - 3, this.currentPage + 3);
                        this.pages.push({
                            number: null
                        });
                        this.makePagesRange(this.pageCount - 1, this.pageCount);

                    } else if (this.currentPage < 7) {
                        this.makePagesRange(1, 8);
                        this.pages.push({
                            number: null
                        });
                        this.makePagesRange(this.pageCount - 1, this.pageCount);

                    } else if (this.currentPage >= this.pageCount - 5) {
                        this.makePagesRange(1, 2);
                        this.pages.push({
                            number: null
                        });
                        this.makePagesRange(this.pageCount - 5, this.pageCount);

                    }
                } else {
                    this.makePagesRange(1, this.pageCount);
                }
            },

            makePagesRange(x,y) {
                for (var i=x;i<=y;i++){
                    this.pages.push({
                        number: i
                    });
                }
            },

            getClass(pageNumber) {
                if (pageNumber == this.currentPage) {
                    return 'active';
                } else if (pageNumber == null) {
                    return 'disabled';
                } else if (pageNumber == 'previous' && this.currentPage == 1) {
                    return 'disabled';
                } else if (pageNumber == 'next' && this.currentPage == this.pageCount) {
                    return 'disabled';
                } else {
                    return 'clickable';
                }
            },

            pageNull(page) {
                return page.number == null;
            },

            triggerPageChange(newPage) {
                if(newPage < 1 || newPage > this.pageCount)
                    return;
                if (this.currentPage != newPage) {
                    this.$emit('page-changed', newPage);
                }
            }
        }
    }
</script>