<template>
	<!-- App -->
	<div id="app" class="theme-deeppurple">
		<!-- Statusbar -->
		<f7-statusbar></f7-statusbar>
		
		<!-- Main Views -->
		<f7-views>
			<f7-view id="main-view" navbar-fixed navbar-through :dynamic-navbar="true" main>
				<!-- Navbar -->
				<f7-navbar class="title-bar">
					<f7-nav-center sliding>看天气</f7-nav-center>
				</f7-navbar>
				<!-- Pages -->
				<f7-pages>
					<f7-page>
            <f7-list accordion>
              <f7-list-item accordion-item class="accordion-item-expanded" title="北京">
                <f7-accordion-content>
                  <f7-block>
                    <p>
                      28° 晴
                    </p>
                    <img src="//s1.sencdn.com/web/icons/3d_50/0.png" />
                  </f7-block>
                </f7-accordion-content>
              </f7-list-item>
              <f7-list-item accordion-item title="上海">
                <f7-accordion-content>
                  <f7-block>
                    <p>
                      28° 雨
                    </p>
                    <img src="//s1.sencdn.com/web/icons/3d_50/25.png" />
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
  require('framework7/dist/css/framework7.material.css');
  require('framework7/dist/css/framework7.material.colors.css');

  const crypto = require('crypto')
  const $ = require('jquery')

  // https://api.seniverse.com/v3/weather/now.json?key=spubq0leuz0kccjg&location=beijing&language=zh-Hans&unit=c
  // var url = 'https://api.seniverse.com/v3/weather/now.json?key=spubq0leuz0kccjg&location=beijing&language=zh-Hans&unit=c'
  // $.ajax({
  //   url
  // }).done(data => {
  //   // alert(JSON.stringify(data))
  // }).fail(e => {
  //   // alert(JSON.stringify(e))
  // })

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
      getData (location, callback) {
        var mockData = {"results":[{"location":{"id":"WX4FBXXFKE4F","name":"北京","country":"CN","path":"北京,北京,中国","timezone":"Asia/Shanghai","timezone_offset":"+08:00"},"now":{"text":"阴","code":"9","temperature":"17"},"last_update":"2017-10-17T14:00:00+08:00"}]}
         callback(mockData)
      }
    }
  }
</script>
<style lang="sass">
.navbar .searchbar:after{
  display: none;
}
.accordion-item {
  .item-inner {
     background: none!important;
  }
  img {
    display: none;
  }
}
.accordion-item-expanded {
  background: #673ab7;
  color: #fff;
  opacity: .9;
  &:after {
    content: '';
    display: block;
    height: 1px;
    overflow: hidden;
    clear: both;
  }
  .item-inner {
    .item-title {
      &:after {
        background: #fff;
      }
    }
  }
  .accordion-item-content {
    float: left;
    width: 100%;
    overflow: visible;
  }
  img {
    display: block;
    position: absolute;
    top: -35px;
    right: 15px;
  }
  .content-block {
    color: #fff;
  }
}
</style>
