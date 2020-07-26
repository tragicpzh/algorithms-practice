<template>
    <div class="container-fluid">
        <div class="card">
            <div class="card-header">
                <h2>操作</h2>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col-md-2">
                        <button class="btn btn-primary" v-on:click="stackPush">插入</button>
                    </div>
                    <div class="col-md-2">
                        <input class="input-group input-group-text" type="text" id="push_element" v-model="push_element">
                    </div>
                    <div class="col-md-2">
                        <button class="btn btn-danger" v-on:click="stackDel">删除</button>
                    </div>
                    <div class="col-md-2">
                        <button class="btn btn-danger" v-on:click="stackReset">清空</button>
                    </div>
                    <div class="col-md-2">
                        <button class="btn btn-primary" v-on:click="stackSize">大小</button>
                    </div>
                    <div class="col-md-2">
                        <button class="btn btn-primary" v-on:click="stackCheck">查看</button>
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
        name: "stack",
        data(){
            return{
                push_element:'请输入插入的元素',
                stack:[],
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
            stackPush(){
                this.stack.push(this.push_element)
                this.track+="push  "+this.push_element+"\n"
            },
            stackDel(){
                this.track+="pop  "+this.stack.pop()+"\n"
            },
            stackReset(){
                this.stack=[]
                this.track+="Reset"+"\n"
            },
            stackSize(){
                this.track+="Size  "+this.stack.length+"\n"
            },
            stackCheck(){
                this.track+="Check(栈顶到栈底)"+"\n"
                for(let i=this.stack.length-1;i>=0;i--){
                    this.track+=this.stack[i]+"\n"
                }
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