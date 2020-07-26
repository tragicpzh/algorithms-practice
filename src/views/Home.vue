<template>
    <div class="container-fluid ">
        <div class="row">
            <div class="clo-md-2 nav-style" style="display: inline-block">
              <div class="nav ">
                <ul class="nav nav-pills flex-column border border-dark border-circle">
                    <li class="nav-item dropdown dropright">
                        <a class="dropdown-toggle" data-toggle="dropdown" href="#" >算法</a>
                        <ul class="nav-item dropdown-menu">
                            <li class="nav-item">
                                <a class="nav-link"  href="#" v-on:click="onLoadBody('fastsort')">快速排序</a>
                            </li>
                            <li class="nav-item">
                                <a class="nav-link"  href="#" v-on:click="onLoadBody('mergesort')">归并排序</a>
                            </li>
                            <li>
                                <a class="nav-link" href="#" v-on:click="onLoadBody('shellsort')">希尔排序</a>
                            </li>
                        </ul>
                    </li>
                    <li class="nav-item">
                      <a class="nav-link"  href="#">数据结构</a>
                    </li>
                    <li class="nav-item">
                      <a class="nav-link"  href="#">图论</a>
                    </li>
                    <li class="nav-item">
                      <a class="nav-link"  href="#">数论</a>
                    </li>
                    <li class="nav-item">
                      <a class="nav-link"  href="#">字符串</a>
                    </li>
                  </ul>
              </div>
            </div>
           <div class="col-md-10" style="display: inline-block">
                <div :is='com' ></div>
           </div>
        </div>
    </div>
</template>
<script>
import fastsort from '../components/fast-sort';
import appindex from "@/components/appindex";
import mergesort from '../components/merge-sort';
import shellsort from '../components/shell-sort';
    export default {
      name: 'Home',
      data(){
          return {
              com:'appindex'
          }
      },
      methods:{
          onLoadBody:function(id){
              this.com=id;
              console.log(this.com);
          }
      },
      components: {
          fastsort,
          appindex,
          mergesort,
          shellsort
      }
    }
</script>
<style>
    .nav-style{
        font-family: "Adobe 楷体 Std R";
        font-size: 200%;
    }
    .nav-style a{
        color: black;
    }
    .border-circle{
        border-radius: 2rem;
    }
</style>
