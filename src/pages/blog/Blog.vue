<template>
  <div>
    <div id="loader"></div>
    <div class="header-nav">
      <ul id="menu-menu" class="menu">
        <li id="menu-item-2561" class="pview menu-item menu-item-type-post_type menu-item-object-page menu-item-2561"><a
          class="pviewa" @click="goNext(0)">博文</a></li>
        <li id="menu-item-4084" class="pview menu-item menu-item-type-post_type menu-item-object-page menu-item-4084"><a
          class="pviewa" @click="goNext(1)">生活驿站</a></li>
        <li id="menu-item-4910" class="pview menu-item menu-item-type-post_type menu-item-object-page menu-item-4910"><a
          class="pviewa" @click="goNext(2)">建站笔录</a></li>
        <li id="menu-item-3507" class="menu-item menu-item-type-custom menu-item-object-custom menu-item-3507">
          <a class="pviewa" @click="goNext(3)">留言板</a></li>
        <li id="menu-item-6509" class="menu-item menu-item-type-custom menu-item-object-custom menu-item-6509">
          <a class="pviewa" @click="goNext(4)">关于我</a></li>
      </ul>
      <p>© 2018 素锦. Powered by WordPress</p>
    </div>

    <div id="container">
      <div id="screen">
        <div id="mark"
             style="height: 612px; position: relative; transform: translate3d(0px, 0px, 0px); transform-style: preserve-3d; backface-visibility: hidden;">
          <div class="layer" data-depth="0.4"
               style="width: 1518.45px; height: 680.3px; margin-left: -76.2237px; margin-top: -34.15px; position: relative; display: block; left: 0px; top: 0px; transform: translate3d(-7.1104px, -23.9074px, 0px); transform-style: preserve-3d; backface-visibility: hidden;">
            <img id="cover" crossorigin="anonymous" src="../../assets/images/wallhaven-630537.jpg" width="2560"
                 height="1440" style="width: 1518px; height: 853.875px; left: 0px; top: -86.5px;">
          </div>
        </div>

        <div id="vibrant">
          <svg viewBox="0 0 2880 1620" height="100%" preserveAspectRatio="xMaxYMax slice">
            <polygon opacity="0.7" points="2000,1620 0,1620 0,0 600,0 " style="fill: rgb(137, 5, 37);"></polygon>
          </svg>
          <div style="background-color: rgb(137, 5, 37);"></div>
        </div>

        <div id="header">
          <div>
            <a class="icon-logo"></a>
            <div class="icon-menu switchmenu" style="color: rgb(192, 9, 81);" @click="switchMenu()"></div>
          </div>
        </div>
        <div id="post0">
          <p>四月 13, 2018</p>
          <h2><a data-id="6478" class="posttitle">浮生若梦，为欢几何</a></h2>
          <p style="line-height:30px;font-size:12px;">我曾将青春翻涌成她<br>
            也曾指尖弹出盛夏<br>
            心之所动<br>
            且就随缘去吧<br></p>
        </div>
      </div>
      <div id="primary">
        <div class="post" v-for="(blog, index) in blogList" :key="index" @click="goArticleDetail(blog.articleId)">
          <a data-id="6429" :title="blog.articleTitle">
            <img width="680" height="440"
                 :src="$util.imgPath(blog.articleImg)"
                 class="cover">
          </a>
          <div class="else">
            <p class="create-time-p">{{blog.createTime | timeFilter}}</p>
            <h3><a data-id="6429" class="posttitle">{{blog.articleTitle}}</a></h3>
            <p class="blog-desc" v-html="blog.articleDesc"></p>
            <p class="here">
              <span class="icon-letter">{{blog.commentsNum}}</span>
              <span class="icon-view">{{blog.viewNum}}</span>
              <a class="likeThis" id="like-6429"><span class="icon-like"></span><span
                class="count">{{blog.favoriteNum}}</span></a></p>
          </div>
        </div>
        <div id="pager" v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="10"><a
          class="more">滚动加载更多</a></div>
      </div>
    </div>

    <div id="preview" class="trans" style="min-height: 612px;"></div>
  </div>

</template>

<script>
  import imgPath from '../../common/img/imgPath';
  import {getFormatDateByLong} from '../../assets/js/date-format'
  import '../../assets/styles/Diaspora.css'

  export default {
    data() {
      return {
        blogList: [],
        page: 0,
        imgPath: imgPath,
        busy: false,
        loadingShow: false,
        pageCount: 0
      }
    },
    filters: {
      typeFilter(type) {
        return type ? type === 1 ? '生活驿站' : '轻松时刻' : '技术杂谈'
      },
      timeFilter(time) {
        return getFormatDateByLong(time, 'yyyy-MM-dd');
      }
    },
    methods: {
      switchMenu() {
        window.scrollTo(0, 0)
        if (document.body.className !== 'touch') {
          document.body.className = 'touch'
          document.getElementsByTagName('html')[0].className = 'touch'
          return
        }
        document.body.className = 'mu'
        document.getElementsByTagName('html')[0].className = 'mu'
      },
      goArticleDetail(articleId) {
        this.$router.push('/article-detail/' + articleId);
      },
      loadMore() {
        this.$nextTick(function () {
          if (this.page < this.pageCount - 1) {
            this.busy = true;
            this.page++;
            setTimeout(() => {
              this.queryMoreBlog();
            }, 500);
          } else {
            document.getElementsByClassName('more')[0].innerHTML = '没有更多了...'
          }
        });
      },
      goNext(index) {
        switch (index) {
          case 0:
            location.href = location.href.replace(/(#\/).*/g, '$1blog-list');
            return;
          case 1:
            location.href = location.href.replace(/(#\/).*/g, '$1blog');
            return;
          case 2:
            location.href = location.href.replace(/(#\/).*/g, '$1websiteHistory');
            return;
          case 3:
            location.href = location.href.replace(/(#\/).*/g, '$1messageBoard');
            return;
          case 4:
            location.href = location.href.replace(/(#\/).*/g, '$1aboutMe');
            return;
          default:
            ;
        }
      },
      queryBlogListByPage() {
        this.$http.api({
          url: '/blog/init',
          params: {page: this.page, dist: 0},
          successCallback: function (data) {
            this.blogList = data.articleList;
            this.pageCount = data.pageCount;
          }.bind(this)
        });
      },
      queryMoreBlog() {
        this.$http.api({
          url: '/blog/init',
          params: {page: this.page, dist: 0},
          successCallback: function (data) {
            this.busy = false;
            this.blogList = this.blogList.concat(data.articleList);
          }.bind(this)
        });
      }
    },
    created() {
      this.queryBlogListByPage()
    },
    mounted () {
      require('../../assets/js/Diaspora');
    },
    beforeRouteLeave (to, from, next) {
      document.body.className = 'touch'
      document.getElementsByTagName('html')[0].className = 'touch'
      next()
    },
    components: {
    }
  }
</script>

<style lang="less" rel="stylesheet/less" scoped>
  .cover {
    cursor: pointer;
    transition: all 0.4s;
    &:hover {
      transform: scale(1.1);
    }
  }

  .blog-desc {
    height: 100px;
    line-height: 25px;
    overflow: hidden;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
  }
</style>
