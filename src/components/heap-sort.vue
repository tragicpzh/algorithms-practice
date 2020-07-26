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
                            <button class="btn btn-primary" v-on:click="heap_sort_rand">排序</button>
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
                            <button class="btn btn-primary" v-on:click="heap_sort_flex">排序</button>
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
                        <th>排序次数</th>
                        <th>排序前</th>
                        <th>排序后</th>
                        <th>有序区</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(sorts,index) in sort_datas">
                        <td>{{index+1}}</td>
                        <td>
                            <tr v-for="(row) in sorts.before_data">
                                <td>{{row}}</td>
                            </tr>
                        </td>
                    <td>
                        <tr v-for="(row) in sorts.after_data">
                            <td>{{row}}</td>
                        </tr>
                    </td>
                        <td>{{sorts.after_sort}}</td>
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
        name: "heap-sort",
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
            heap_sort_flex() {
                let arr = this.sort_data.split(",");
                this.sort_final = this.heap_sort(arr).join(',');
            },
            heap_sort_rand() {
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
                this.sort_final = this.heap_sort(arr).join(',');
                console.log(this.sort_final);
            },
            heap_sort(v) {
                var arr = (v.slice(0)).map(v => parseInt(v));
                var length=arr.length;
                this.sort_datas = [];
                var track = this.sort_datas;
                if(length<1)return arr
                function swap(i,j){
                    let tmp=arr[i]
                    arr[i]=arr[j]
                    arr[j]=tmp
                }
                function buildHeap(){
                    for(let i=length-1;i>=0;i--) {
                        console.log(i)
                        adjust(i)
                    }
                }
                function adjust(index){
                    let maxindex=index
                    if(index*2+1<length&&arr[index*2+1]>arr[maxindex])maxindex=index*2+1
                    if(index*2+2<length&&arr[index*2+2]>arr[maxindex])maxindex=index*2+2
                    if(maxindex!=index){
                        swap(index,maxindex)
                        adjust(maxindex)
                    }
                    else return
                }
                buildHeap()
                while(length>0){
                    let i=-1
                    let j=0
                    let obj={
                        before_data:[],
                        after_data:[]
                    }
                    while(j<length){
                        let tmp=arr.slice(i+1,j+1)
                        obj.before_data.push(tmp)
                        if(j===length-1)break
                        i=j
                       j=(j*2+2<length)?j*2+2:length-1
                    }
                    swap(0,length-1)
                    length--
                    adjust(0)
                    i=-1
                    j=0
                    while(j<length){
                        let tmp=arr.slice(i+1,j+1)
                        obj.after_data.push(tmp)
                        if(j===length-1)break
                        i=j
                        j=(j*2+2<length)?j*2+2:length-1
                    }
                    obj.after_sort=arr.slice(length,arr.length)
                    track.push(obj)
                }
                return arr
            }
        }
    }
</script>

<style scoped>

</style>