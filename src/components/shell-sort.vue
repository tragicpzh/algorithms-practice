<template>
    <div class="container-fluid">
        <div class="card">
            <div class="card-header">
                <h2>随机数排序</h2>
            </div>
            <div class="card-body">
                <div class="form-row">
                    <div class="form-group col-md-3">
                        <label style="float: left" htmlFor="rand-min">最小值</label>
                        <input type="number" class="form-control" placeholder="0" id="rand-min" v-model="rand_min">
                    </div>
                    <div class="form-group col-md-3">
                        <label style="float: left" htmlFor="rand-max">最大值</label>
                        <input type="number" class="form-control" placeholder="100" id="rand-max"
                               v-model="rand_max">
                    </div>
                    <div class="form-group col-md-3">
                        <label style="float: left" htmlFor="rand-size">数量</label>
                        <input type="number" class="form-control" placeholder="0" id="rand-size"
                               v-model="rand_size">
                    </div>
                    <div class="form-group col-md-3">
                        <div style="margin-top: 15%;width: 50%;display: inline-block;float: left">
                            <button class="btn btn-outline-dark" v-on:click="reSet">重置</button>
                        </div>
                        <div style="margin-top: 15%;width: 50%;display: inline-block;float: left">
                            <button class="btn btn-primary" v-on:click="shell_sort_rand">排序</button>
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
                        <label htmlFor="sort-num" style="float: left">排序内容</label>
                        <textarea id="sort-num" class="form-control" autoHeight="true"
                                  v-model="sort_data"></textarea>
                    </div>
                    <div class="form-group col-md-4">
                        <div style="margin-top: 20%">
                            <button class="btn btn-primary" v-on:click="shell_sort_flex">排序</button>
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
                        <th>增量次数</th>
                        <th>合并前</th>
                        <th>合并后</th>
                        <th>分组</th>
                        <th>gap</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(sorts,index) in sort_datas">
                        <td>{{index+1}}</td>
                        <td>{{sorts.before_data}}</td>
                        <td>{{sorts.after_data}}</td>
                        <td>{{sorts.groups}}</td>
                        <td>{{sorts.gap}}</td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
    import $ from 'jquery'
    $(function () {
        $.fn.autoHeight = function () {
            function autoHeight(elem) {
                elem.style.height = 'auto';
                elem.scrollTop = 0; //防抖动
                elem.style.height = elem.scrollHeight + 'px';
            }

            this.each(function () {
                autoHeight(this);
                $(this).on('keyup', function () {
                    autoHeight(this);
                });
            });
        }
        $('textarea[autoHeight]').autoHeight();
    })
    export default {
        name: "shell-sort",
        data() {
            return {
                rand_min: 0,
                rand_max: 100,
                rand_size: 0,
                sort_data: '请输入数据',
                sort_datas: [],
                sort_final: ''
            }
        },
        methods: {
            reSet() {
                this.rand_min = 0;
                this.rand_max = 100;
                this.rand_size = 0;
            },
            shell_sort_flex() {
                let arr = this.sort_data.split(",");
                this.sort_final = this.shell_sort(arr).join(',');
            },
            shell_sort_rand() {
                let cnt = this.rand_size;
                let arr = [];
                let size = this.rand_max - this.rand_min;
                console.log(this.rand_min);
                console.log(this.rand_max);
                console.log(size);
                while (cnt--) {
                    let tmp = Math.floor(Math.random() * (size + 1));
                    arr.push(tmp + parseInt(this.rand_min));
                }
                console.log(arr);
                this.sort_final = this.shell_sort(arr).join(',');
                console.log(this.sort_final);
            },
            shell_sort(v) {
                let arr = (v.slice(0)).map(v => parseInt(v));
                this.sort_datas = [];
                var track = this.sort_datas;
                return (function f(v) {
                    let arr=v
                    let length=arr.length
                    let gap=Math.floor(length/2)
                    while(gap>0){
                        console.log(gap)
                        var obj={
                            before_data:arr,
                            gap:gap,
                            groups:[]
                        }
                        for(let i=0;i<gap;i++){
                            let j=i
                            let group=[]
                            while(j<length){
                                group.push(arr[j])
                                j+=gap
                            }
                            obj.groups.push(group)
                        }
                        for(let i=gap;i<length;i++){
                            let tmp=arr[i]
                            let preindex=i-gap
                            while(preindex>=0&&arr[preindex]>tmp){
                                arr[preindex+gap]=arr[preindex]
                                preindex-=gap
                            }
                            arr[preindex+gap]=tmp
                        }
                        obj.after_data=arr
                        track.push(obj)
                        gap=Math.floor(gap/2)
                    }
                    return arr;
                })(arr);
            }
        }
    }
</script>

<style scoped>

</style>