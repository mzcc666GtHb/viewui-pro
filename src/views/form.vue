<template>
    <div>
        <Form ref="form" :model="form" :rules="rules">
            <Table :columns="columns" :data="form.data">
                <template slot-scope="{ row, index }" slot="age">
                    <FormItem :rules="rules.age"
                              :prop="`data.${index}.age`">
                        <Input type="text" v-model="form.data[index].age"/>
                    </FormItem>
                </template>
            </Table>
        </Form>
        <Button @click="validate">校验</Button>
        <Button @click="validate">校验</Button>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                rules: {
                    age: [
                        {required: true, type: 'string', message: '请输入', trigger: 'blur'}
                    ]
                },
                columns: [
                    {
                        title: '姓名',
                        key: 'name'
                    },
                    {
                        title: '年龄',
                        slot: 'age',
                        key: 'age'
                    },
                    {
                        title: '地址',
                        key: 'address'
                    },
                ],
                form: {
                    data: [
                        {
                            name: '给',
                            age: '123',
                            address: '北京市'
                        },
                        {
                            name: '发',
                            age: '123',
                            address: '北京市'
                        },
                        {
                            name: '吖',
                            age: 30,
                            address: '上海市'
                        },
                        {
                            name: '从',
                            age: 26,
                            address: '深圳市'
                        }
                    ],
                },
            }
        },
        methods: {
            validate() {
                this.$refs.form.validate(valid => {
                    if (valid) {
                        this.$Message.success('Success!')
                    } else {
                        this.$Message.error('error!')
                    }
                })
            },
        }
    }
</script>
