<template>
    <div class="app-card bg-white border pr-0 flex-shrink-0 mb-4 py-4 d-flex justify-content-between position-relative">
        <div v-if="card.filtered" @click="unsetFilter" class="app-filtered-overlay bg-white position-absolute"></div>
        <div class="h-100 pl-3 pr-1 px-lg-4 pr-sm-4 d-flex align-items-center">
            <b-form-checkbox size="lg" v-model="card.checked"></b-form-checkbox>
        </div>

        <div class="d-flex flex-column flex-grow-1">
            <div class="d-flex justify-content-between mb-1 align-items-baseline flex-wrap pr-3 pr-lg-0">
                <div class="app-dard-price text-danger flex-shrink-0">
                    {{ card.price }}
                </div>
                <div class="app-card-type d-flex align-items-center flex-shrink-0 py-1 px-2 rounded-pill border" :id="'tooltip-'+num">
                    <div class="app-card-type__icon flex-shrink-0 d-flex justify-content-center align-items-center overflow-hidden mr-2">
                        <b-img-lazy :src="icon" class="flex-shrink-0"></b-img-lazy>
                    </div>
                    <span>
                        {{ card.type }}
                    </span>
                </div>
                <b-tooltip :target="'tooltip-'+num" placement="bottom">
                    {{ card.tooltip }}
                </b-tooltip>                
                <div 
                    :class="'app-filter-button flex-shrink-0 rounded-0 py-1 px-2 d-flex align-items-center mr-0 mr-lg-4 alert alert-'+card.status.variant" 
                    size="sm"
                    @click="cardFilter"
                >
                    <span style="font-size:18px">&bull;&nbsp;&nbsp;</span>{{ card.status.text }}
                </div>            
            </div>
            <div class="app-object-info d-flex flex-wrap pr-5 mb-2">
                <span class="text-black text-left">
                    {{ card.name }}, 
                </span>
                <span class="text-secondary" style="opacity: 0.8;">
                    &nbsp;{{ card.address }},
                </span>            
                <span class="text-secondary" style="opacity: 0.8;">
                    &nbsp;{{ card.date }}
                </span>
            </div>
            <div class="d-flex justify-content-between pr-3 pr-lg-5">
                <div class="d-flex justify-content-between align-items-baseline flex-shrink-1 flex-shrink-sm-0 flex-grow-1 pt-4">
                    <div class="d-flex flex-column border-right text-secondary w-50">
                        <div class="mb-2 text-black text-left">
                            {{ card.number }}
                        </div>
                        <div v-if="card.rooms" class="text-black text-left">
                            {{ card.rooms }}
                        </div>                
                    </div>
                    <!-- -------------- -->
                    <div class="d-flex flex-column border-right text-secondary w-50">
                        <div class="mb-2 text-black text-left pl-2 pl-sm-4">
                            {{ card.footage }}
                        </div>
                        <div v-if="card.floor" class="text-black text-left pl-4">
                            {{ card.floor }}
                        </div>                
                    </div>                
                </div>
                <div class="app-card-img d-flex jusify-content-center flex-shrink-0 align-items-center overflow-hidden">
                    <b-img-lazy :src="img"></b-img-lazy>
                </div>
            </div>
            <div class="d-flex justify-content-between align-items-baseline pr-5 mt-2">
                <div class="app-geo position-relative text-left">
                    <div class="app-geomark d-flex justify-content-center align-items-center overflow-hidden position-absolute">
                        <b-img-lazy :src="geoMark"></b-img-lazy>
                    </div>
                    <span>
                        {{ card.geo }}
                    </span>
                </div>
                <div class="app-object-date text-secondary">
                    {{ card.published }}
                </div>
            </div>
        </div>
    </div>
    
</template>

<script>
export default {
    props: ['card', 'num'],
    computed: {
        img: function(){
            return require('../assets/'+this.card.image)
        },
        icon: function(){
            return require('../assets/'+this.card.icon)
        },
        geoMark: function() {
            return require('../assets/geo.svg')
        }
    },
    methods: {
        cardFilter: function(){
            this.$emit('cardFilter', this.card.status.code)
        },
        unsetFilter: function(){
            this.$emit('unsetFilter')
        }
    }
}
</script>

<style lang="sass" scoped>
$icon_size: 12px
.app-card-type__icon
    width: $icon_size
    height: $icon_size
    min-width: $icon_size
    max-width: $icon_size
    min-height: $icon_size
    max-height: $icon_size
    background-position: center
    background-repeat: no-repeat
    background-size: 100%
.app-dard-price
    font-size: 15px
    font-weight: 700    
.alert
    border: none !important   
@media (min-width: 992px)     
    .app-card
        width: calc(50% - 16px)
@media (max-width: 991.98px)        
    .app-card
        width: 100%
.app-card-type    
    font-size: 12px
.app-object-info
    font-size: 14px    
.app-geo
    font-size: 14px
    max-width: 300px 
.app-geomark
    background-position: center
    width: 14px
    height: 14px
    position: absolute
    left: -20px
    top: 2px
.app-object-date
    opacity: 0.8
    font-size: 13px    
@media (max-width: 575px)    
    .app-card-img
        width: 124px
.app-filter-button
    cursor: pointer        
.app-filtered-overlay
    top: 0
    left: 0
    right: 0    
    bottom: 0
    z-index: 10
    opacity: 0.8
</style>