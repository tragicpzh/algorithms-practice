<template>
    <div class="container-fluid">
        <div class="card">
            <div class="card-header">
                <h2>随机数排序</h2>
            </div>
            <div class="card-body">
                <div class="form-row">
                    <div class="form-group col-md-3">
                        <label style="float: left" for="rand-min">最小值</label>
                        <input type="number" class="form-control" placeholder="0" id="rand-min" v-model="rand_min">
                    </div>
                    <div class="form-group col-md-3">
                        <label style="float: left" for="rand-max">最大值</label>
                        <input type="number" class="form-control" placeholder="100" id="rand-max" v-model="rand_max">
                    </div>
                    <div class="form-group col-md-3">
                        <label style="float: left" for="rand-size">数量</label>
                        <input type="number" class="form-control" placeholder="0" id="rand-size" v-model="rand_size">
                    </div>
                    <div class="form-group col-md-3">
                        <div style="margin-top: 15%;width: 50%;display: inline-block;float: left">
                            <button class="btn btn-outline-dark" v-on:click="reSet">重置</button>
                        </div>
                        <div style="margin-top: 15%;width: 50%;display: inline-block;float: left">
                            <button class="btn btn-primary" v-on:click="fast_sort_rand">排序</button>
                        </div>

                    </div>
                </div>
            </div>
        </div>
        <div class="card">
            <div class="card-header">
                <h2>固定值排序</h2>
            </div>
            <div class="card-body">
                <div class="form-row">
                    <div class="form-group col-md-8">
                        <label for="sort-num" style="float: left">排序内容</label>
                        <textarea id="sort-num" class="form-control"  autoHeight="true" v-model="sort_data"></textarea>
                    </div>
                    <div class="form-group col-md-4">
                        <div style="margin-top: 20%">
                            <button class="btn btn-primary" v-on:click="fast_sort_flex">排序</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="card">
            <div class="card-header">
                <h2>排序结果</h2>
            </div>
            <div class="card-body">
                <span style="float: left;font-size: 30px">{{sort_final}}</span>
            </div>
            <div class="card-footer">
                <h2></h2>
            </div>
        </div>
        <div class="card">
            <div class="card-header">
                <h2>排序过程</h2>
            </div>
            <div class="card-body">
                <table class="table table-hover">
                    <thead>
                        <tr>
                            <th>划分次数</th>
                            <th>划分前</th>
                            <th>划分后</th>
                            <th>key</th>
                            <th>i,j</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(sorts,index) in sort_datas">
                            <td>{{index+1}}</td>
                            <td>{{sorts.before_data}}</td>
                            <td>{{sorts.after_data}}</td>
                            <td>{{sorts.key}}</td>
                            <td>{{sorts.end_ij}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
    import $ from 'jquery'
    $(function(){
        $.fn.autoHeight = function(){
            function autoHeight(elem){
                elem.style.height = 'auto';
                elem.scrollTop = 0; //防抖动
                elem.style.height = elem.scrollHeight + 'px';
            }
            this.each(function(){
                autoHeight(this);
                $(this).on('keyup', function(){
                    autoHeight(this);
                });
            });
        }
        $('textarea[autoHeight]').autoHeight();
    })
    export default {
        name: "fast-sort",
        data(){
            return{
                rand_min:0,
                rand_max:100,
                rand_size:0,
                sort_data:'请输入数据',
                sort_datas: [],
                sort_final:''
            }
        },
        methods:{
            reSet(){
                this.rand_min=0;
                this.rand_max=100;
                this.rand_size=0;
            },
            fast_sort_flex(){
              let arr=this.sort_data.split(",");
              this.sort_final=this.fast_sort(arr).join(',');
            },
            fast_sort_rand(){
                let cnt=this.rand_size;
                let arr=[];
                let size=this.rand_max-this.rand_min;
                console.log(this.rand_min);
                console.log(this.rand_max);
                console.log(size);
                while(cnt--){
                    let tmp=Math.floor(Math.random()*(size+1));
                    arr.push(tmp+parseInt(this.rand_min));
                }
                console.log(arr);
                this.sort_final=this.fast_sort(arr).join(',');
                console.log(this.sort_final);
            },
            fast_sort(v){
                let arr=v.slice(0);
                this.sort_datas=[];
                var track=this.sort_datas;
                return (function f(v) {
                    let arr=v.slice(0);
                    let key=arr[0];
                    let size=arr.length;
                    if(size==1)return arr;
                    let i=0;
                    let j=size-1;
                    let tmp=0;
                    var obj={
                        before_data:arr.join(',')
                    }
                    while(i!==j){
                        while((arr[j]-key)>=0&&j!==i)j--;
                        if(j===i)break;
                        else {
                            tmp=arr[i];
                            arr[i]=arr[j];
                            arr[j]=tmp;
                        }
                        while((arr[i]-key)<=0&&i!==j)i++;
                        if(i===j)break;
                        else{
                            tmp=arr[i];
                            arr[i]=arr[j];
                            arr[j]=tmp;
                        }
                    }
                    obj.after_data=arr.join(',')
                    obj.key=key
                    obj.end_ij=i
                    track.push(obj);
                    let left=f(arr.slice(0,i+1));
                    let right=(i+1<=size-1)?f(arr.slice(i+1)):[];
                    let end=left.concat(right);
                    return end;
                })(arr)
            }
        }
    }
</script>

<style scoped>

</style>