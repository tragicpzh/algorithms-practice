<template>
    <div class="container-fluid">
        <div class="card">
            <div class="card-header">
                <h2>操作</h2>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col-md-1">
                        <button class="btn btn-primary" v-on:click="Push">插入</button>
                    </div>
                    <div class="col-md-2">
                        <input class="input-group input-group-text" type="text" id="push_element" v-model="push_element">
                    </div>
                    <div class="col-md-1">
                        <input class="input-group input-group-text" type="text" id="push_index" v-model="push_index">
                    </div>
                    <div class="col-md-2">
                        <button class="btn btn-primary" v-on:click="Find">查找</button>
                    </div>
                    <div class="col-md-2">
                        <input class="input-group input-group-text" type="text" id="find_index" v-model="find_index">
                    </div>
                    <div class="col-md-2">
                        <button class="btn btn-danger" v-on:click="Del">删除</button>
                    </div>
                    <div class="col-md-2">
                        <input class="input-group input-group-text" type="text" id="del_index" v-model="del_index">
                    </div>
                </div>
                <br></br>
                <div class="row">
                    <div class="col-md-1" >
                        <button class="btn btn-danger" v-on:click="Reset">清空</button>
                    </div>
                    <div class="col-md-2 el-col-md-offset-6">
                        <button class="btn btn-primary" v-on:click="Size">大小</button>
                    </div>
                    <div class="col-md-2 el-col-md-offset-4">
                        <button class="btn btn-primary" v-on:click="Check">查看</button>
                    </div>
                </div>
            </div>
        </div>
        <div class="card">
            <div class="card-header">
                <h2>操作结果</h2>
            </div>
            <div class="card-body my-textarea">
                <textarea id="track" cols="80" ref="textarea" :style="{'height': height}" v-model="track" class="text-center textarea" readonly="readonly"></textarea>
            </div>
        </div>

    </div>
</template>

<script>
    import calcTextareaHeight from '@/assets/calcTextareaHeight'
    export default {
        name: "linked-list",
        data(){
            return{
                push_element:'请输入插入的元素',
                push_index:'插入的位置',
                find_index:'查找的位置',
                del_index:'删除的位置',
                queue:[],
                track:'',
                height: '30px'
            }
        },
        watch:{
            track(){
                this.getHeight();
            }
        },
        methods:{
            getHeight(){
                this.height=calcTextareaHeight(this.$refs.textarea,1,null).height
            },
            Push(){
                this.queue.push(this.push_element)
                this.track+="push  "+this.push_element+"\n"
            },
            Del(){
                this.track+="pop  "+this.queue.shift()+"\n"
            },
            Reset(){
                this.queue=[]
                this.track+="Reset"+"\n"
            },
            Size(){
                this.track+="Size  "+this.queue.length+"\n"
            },
            Check(){
                this.track+="Check(队头到队尾)"+"\n"
                for(let i=0;i<this.queue.length;i++){
                    this.track+=this.queue[i]+" "
                }
                this.track+="\n"
            }
        }
    }
</script>

<style scoped>
    .my-textarea.textarea {
        display: inline-block;
        width: 400px;
        line-height: 30px;
        font-size: 30px;
        resize: none;
    }
</style>