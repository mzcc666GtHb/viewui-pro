<template>
    <div class="about">
        <Form ref="form" :model="form" :rules="rules">
            <template v-if="form.disabledGroup === '1'">
                <Table :columns="columns" :data="form.partA.data">
                    <template slot-scope="{row,index}" slot="age">
                        <FormItem :rules="rules.age"
                                  :prop="`partA.data.${index}.age`">
                            <Input type="text" v-model="form.partA.data[index].age"/>
                        </FormItem>
                    </template>
                </Table>
            </template>
            <template v-if="form.disabledGroup === '0'">
                <Table :columns="columns" :data="form.partA.data">
                    <template slot-scope="{row,index}" slot="age">
                        <FormItem :rules="rules.age"
                                  :prop="`partA.data.${index}.age`">
                            <Input type="text" v-model="form.partA.data[index].age"/>
                        </FormItem>
                    </template>
                </Table>
            </template>
            <Icon @click="add" type="ios-add-circle-outline"/>
            <Button type="primary" @click="save">保存</Button>
            <RadioGroup v-model="form.disabledGroup" @on-change="changeRadio">
                <Radio :label="item.value" v-for="item in form.groupList" :key="item.value">{{item.label}}</Radio>
            </RadioGroup>
        </Form>


    </div>
</template>
<script>
    export default {
        data() {
            return {
                renderView: false,
                rules: {
                    age: [
                        {validator: this.validateEmail, trigger: 'blur'}
                    ]
                },
                form: {
                    disabledGroup: '',
                    groupList: [
                    {
                      value: '0',
                      label: '非标准模板合同'
                    },
                    {
                      value: '1',
                      label: '标准模板合同'
                    }
                  ],
                    partA: {
                        data: [
                            {
                                name: '给',
                                age: '',
                                address: '北京市'
                            },
                            {
                                name: '发',
                                age: '',
                                address: '北京市'
                            }
                        ]
                    }

                },
                columns: [
                    {
                        title: 'Name',
                        key: 'name'
                    },
                    {
                        title: 'E-mail',
                        slot: 'age',
                        key: 'age'
                    },
                ]
            }
        },
        mounted() {
          class EventEmitter {
            constructor () {
              this.events = {}
            }
            on (eventName,callback) {
              if (this.events[eventName]) {
                this.events[eventName].push(callback)
              } else {
                this.events[eventName] = [callback]
              }
            }
            emit (eventName,param) {
              this.events[eventName] && this.events[eventName].forEach(callback => callback(param))
            }
          }
          let em = new EventEmitter()
          em.on('change',function (data) {
            console.log(data)
          })
          em.on('change1',function (data) {
            console.log(data)
          })
          em.emit('change','更改12312')
          em.emit('change1','22222')
        },
        methods: {
            validateEmail(rule, value, callback) {
              console.log(1231)
              let reg = new RegExp("^[a-z0-9]+([._\\-]*[a-z0-9])*@([a-z0-9]+[-a-z0-9]*[a-z0-9]+.){1,63}[a-z0-9]+$");
                if (!reg.test(value)) {
                    callback(new Error('邮箱格式不正确'));
                } else {
                    callback();
                }
            },
            add() {
                this.form.partA.data.push({
                    name: '',
                    age: 121,
                    address: '12',
                    date: '2016-10-03'
                })
            },
            save() {
                this.$refs['form'].validate((res) => {
                    console.log(res);
                })
            },
           changeRadio (val) {
             this.$refs['form'].resetFields();
             this.form.disabledGroup = val
             // this.rules = JSON.parse(JSON.stringify(this.rules))
           }
        }
    }
</script>
