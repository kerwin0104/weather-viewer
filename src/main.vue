<template>
	<!-- App -->
	<div id="app" class="theme-deeppurple">
		<!-- Statusbar -->
		<f7-statusbar></f7-statusbar>
		
		<!-- Main Views -->
		<f7-views>
			<f7-view id="main-view" navbar-fixed="true" navbar-through :dynamic-navbar="true" main>
				<!-- Navbar -->
				<f7-navbar class="title-bar">
					<f7-nav-center sliding>看天气</f7-nav-center>
				</f7-navbar>
				<!-- Pages -->
				<f7-pages>
					<f7-page>

            <div class="content-block-title">城市列表</div>
            <f7-list accordion>
              <f7-list-item swipeout accordion-item class="accordion-item-expanded" title="北京">
                <f7-swipeout-actions>
                  <f7-swipeout-button delete>删除</f7-swipeout-button>
                </f7-swipeout-actions>
                <f7-accordion-content>
                  <f7-block>
                    <p>晴</p>
                  </f7-block>
                </f7-accordion-content>
              </f7-list-item>
              <f7-list-item swipeout  accordion-item title="上海">
                <f7-swipeout-actions>
                  <f7-swipeout-button delete>删除</f7-swipeout-button>
                </f7-swipeout-actions>
                <f7-accordion-content>
                  <f7-block>
                    <p>Accordion Item 2 Content</p>
                  </f7-block>
                </f7-accordion-content>
              </f7-list-item>
            </f7-list>

            <f7-fab class="open-popup" data-popup="#popup">
              <f7-icon icon="icon-plus">+</f7-icon>
            </f7-fab>
					</f7-page>
				</f7-pages>
			</f7-view>
		</f7-views>
		
		<!-- Popup -->
		<f7-popup id="popup">
			<f7-view navbar-fixed>
				<f7-pages>
					<f7-page>
						<f7-navbar class="title-bar">
              <f7-searchbar search-list="#search-list" placeholder="请输入城市"></f7-searchbar>
							<f7-nav-right>
								<f7-link :close-popup="true">取消</f7-link>
							</f7-nav-right>
						</f7-navbar>


            <f7-block class="searchbar-not-found">
              未找到该城市
            </f7-block>

            <f7-list
              id="search-list"
              class="searchbar-found"
              v-show="items.length"
              virtual
              :virtual-items="items"
              :virtual-search-all="searchAll"
              >
              <!-- Templte 7 Virtual List Item Template -->
              <t7-template>
                <f7-list-item :title="'{{title}}'"></f7-list-item>
              </t7-template>
            </f7-list>


					</f7-page>
				</f7-pages>
			</f7-view>
		</f7-popup>
	</div>
</template>

<script>
  // if (DeviceInfo.isAndroid) {
  //   require('framework7/dist/css/framework7.material.css');
  //   require('framework7/dist/css/framework7.material.colors.css');
  //   require('framework7-icons/css/framework7-icons.css');
  // } else {
    // require('framework7/dist/css/framework7.ios.css');
    // require('framework7/dist/css/framework7.ios.colors.css');
    // require('framework7-icons/css/framework7-icons.css');
  // }
  require('framework7/dist/css/framework7.material.css');
  require('framework7/dist/css/framework7.material.colors.css');

  const crypto = require('crypto')
  const $ = require('jquery')

  // https://api.seniverse.com/v3/weather/now.json?key=spubq0leuz0kccjg&location=beijing&language=zh-Hans&unit=c
  var url = 'https://api.seniverse.com/v3/weather/now.json?key=spubq0leuz0kccjg&location=beijing&language=zh-Hans&unit=c'
  $.ajax({
    url
  }).done(data => {
    // alert(JSON.stringify(data))
  }).fail(e => {
    // alert(JSON.stringify(e))
  })

	export default {
    data () {
      var items = []
      items.push({
        title: '北京',
        subtitle: '北京'
      })
      return {
        items
      }
    },
    methods: {
      searchAll (query) {
        var self = this;
        var found = [];
        for (var i = 0; i < self.items.length; i++) {
            if (self.items[i].title.indexOf(query) >= 0 || query.trim() === '') found.push(i);
        }
        return found;
      },
      onSwipeoutDeleted () {
      }
    }
  }
</script>
<style>
.navbar .searchbar:after{
  display: none;
}
</style>
