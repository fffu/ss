# 益装网api需求

## 注册 （需要实现返回Token）

### 返回数据

    {
        // ... status, message
        "content": {
            "token": String, // 用于自动登录
        }
    }

## 登录 （已经有了）

## 获取行政区列表（省市区联动） （已经有了）

## 上传图片

### 场景

用户创建任务，设计师上传设计稿，工地上传施工图等

### 获取方式

    POST

### 参数

#### header

    token

#### query

    {
        'scene': Int, // 图片场景；0：用户发布订单时上传户型图；1：设计师上传稿件时的方案图
        'part_id': Int, // 房间id；
        'iamge_type': String, // 'image' || 'MP4'
        'width': Int, // 图片宽度 px
        'height': Int, // 图片高度 px
        'size': Int, // 图片大小 bye
        'info': String, // 设计师对房间设计的描述
        'process_id': Int, // 进度id；为空时新建进度；
    }

### 说明

每次调用接口，仅上传一张图片，需要根据processID关联该进度所有空间图片

### 返回数据

    {
        // ... status, message
        'content': {
            'content': {
                'id': Int, // 如果请求体里面的secne为户型图，返回订单id；如果为方案图，返回进度id；
                'img_id': Int // 图片ID
                'ori_path': // 原始图路径
                'big_path': // 大图路径
                'mid_path': // 中图路径
                'sml_path': // 小图路径
            }
        }
    }

## 删除图片

### 场景

用户上传图片时，撤回操作

### 获取方式

    POST

### 参数

#### header

    token

#### query

    {
        'img_id': Int // 图片ID
    }

### 返回数据

    {
        // ... status, message
    }

## （用  户）创建订单

### 场景

用户提交设计需求

### 获取方式

    POST

### 参数

#### header

    token

#### query

    {
        'type': Int, // 0||缺省(免费设计), 1(付费)
        'designerID' Int, // 设计师ID，如果有此项，表示预约特定设计师
        'area': Number, // 房屋面积
        'unit': String, // 户型
        'village': String, // 所属小区
        'province_id': Int, // 省份ID
        'city_id': Int, // 市ID
        'district_id': Int, // 区县ID
        'budget': Number, // 设计预算
        'image_id': Int, // 户型图ID
        'task_id': Int, // 订单ID
        'info': String // 用户附加输入的需求详情
    }
    
### 返回数据

    {
        //... status，message
    }

## （用  户）完善订单信息（未定）

### 场景

客户创建订单（任务）后，填写的个人生活习惯等信息，提供给设计师

### 获取方式

    POST

### 参数

#### header

    token

#### query

    {
        'members': [  // 家庭成员 Array
            {
                'name': String, // 名字
                'age': Int, // 年龄
                'style': String, // 倾向风格
                'interest': String, // 爱好
                'color': String, // 偏爱颜色
                'info': String, // 备注
            },
            // ……
        ],
        'coustoms': {
            'intercourse': 
        }
    }

## （任务大厅）获取订单列表

### 场景

任务大厅展示

### 获取方式

    GET

### 参数

    {
        'page': Int, // 分页参数
        'size': Int, // 每页数据量
        'order_status': Int, // 按订单状态筛选；默认为空，不筛选；1为招标中；2为已结标；
        'sort_by_time': Int, // 按审核时间排序；默认为空，不排序；1为倒序；2为正序；
        'sort_by_level': Int, // 按订单等级排序；默认为空，不排序；1为倒序；2为正序；
        'level': Int, // 按等级筛选；默认为空，不筛选；1，2，3，4，5，6
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'task_list': [
                {
                    'floor_plan': { // 户型图
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    },
                    'id':
                    'sn': // 订单编号
                    'level': // 订单等级，1，2，3，4，5，6
                    'budget': // 设计预算
                    'unit': // 户型
                    'area': // 面积
                    'style': // 风格
                    'house_status': // '旧房翻新'||'新房'
                    'hosue_type': // '跃层'||'平层'||'别墅'
                    'village': // 小区名
                    'province': // 省份
                    'city': // 市
                    'region': // 区
                    'release_time': // 审核通过时间
                    'create_tiem': // 创建时间
                    'valid_time': // 订单到期时间
                    'designer_list':[ // 已经抢单的设计师
                        {
                            'id':
                            'name':
                            'avatar':{ // 头像
                                'ori_path': // 原始图路径
                                'big_path': // 大图路径
                                'mid_path': // 中图路径
                                'sml_path': // 小图路径
                            },
                            'level':
                        },
                        // ...
                    ],
                },
                / ...
            ]
        }
    }

## （任务大厅）获取订单详情

### 场景

用户任务大厅选择单个订单了解详情

### 获取方式

    GET

### 参数

    {
        'id': // 订单id
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'content': [
                {
                    'layout_pic': { // 户型图路径
                        'ori_path':
                        'big_path':
                        'mid_path':
                        'sml_path':
                    },
                    'id':
                    'sn': // 订单编号
                    'level': // 订单等级，1，2，3，4，5，6
                    'budget': // 设计预算
                    'unit': // 户型
                    'area': // 面积
                    'style': // 风格
                    'house_status': // '旧房翻新'||'新房'
                    'hosue_type': // '跃层'||'平层'||'别墅'
                    'village': // 小区名
                    'province': // 省份
                    'city': // 市
                    'region': // 区
                    'release_time': // 审核通过时间
                    'create_tiem': // 创建时间
                    'valid_time': // 订单到期时间
                    'designers':[ // 已经抢单的设计师
                        {
                            'id':
                            'name':
                            'avatar':{ // 头像
                                'ori_path':
                                'big_path':
                                'mid_path':
                                'sml_path':
                            },
                            'level':
                        },
                        // ...
                    ],
                    'user_habit':{} // 用户完善订单的内容
                    'info': // 用户的补充说明
                },
                / ...
            ]
        }
    }

## （设计师）抢单

### 场景

设计师抢单

### 获取方式

    POST

### 参数

#### header

    token

#### query

    {
        'id': // 订单id
    }

### 返回数据

    {
        // ... status, messages
    }

### 说明

- 后台需要判断登录、身份验证（只有设计师能够抢）、设计师的等级符合订单等级要求、排重、是否达到最大抢单人数
- 冻结状态的设计师不能抢单

## （设计师）获取订单列表

### 场景

设计师个人中心，查看我的订单

### 获取方式

    GET

### 参数

#### header

    token

#### query

    {
        'status': // 空：全部；1：订单进行中；2：订单已完结；此状态需从设计师与订单关联表中查看
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'content': [
                {
                    'layout_pic': { // 户型图路径
                        'ori_path':
                        'big_path':
                        'mid_path':
                        'sml_path':
                    },
                    'id':
                    'sn': // 订单编号
                    'level': // 订单等级，1，2，3，4，5，6
                    'budget': // 设计预算
                    'unit': // 户型
                    'area': // 面积
                    'style': // 风格
                    'house_status': // '旧房翻新'||'新房'
                    'hosue_type': // '跃层'||'平层'||'别墅'
                    'village': // 小区名
                    'province': // 省份
                    'city': // 市
                    'region': // 区
                    'release_time': // 审核通过时间
                    'create_tiem': // 创建时间
                    'valid_time': // 订单到期时间
                    'process_status': // 向设计师展示当前进度描述的文字
                },
                / ...
            ]
        }
    }

## （用  户）获取订单列表

### 场景

用户个人中心，查看我的订单

### 获取方式

    GET

### 参数

#### header

    token

### 返回数据

    {
        // ... status, message
        'content': {
            'content': [
                {
                    'layout_pic': { // 户型图路径
                        'ori_path':
                        'big_path':
                        'mid_path':
                        'sml_path':
                    },
                    'id':
                    'sn': // 订单编号
                    'level': // 订单等级，1，2，3，4，5，6
                    'budget': // 设计预算
                    'unit': // 户型
                    'area': // 面积
                    'style': // 风格
                    'house_status': // '旧房翻新'||'新房'
                    'hosue_type': // '跃层'||'平层'||'别墅'
                    'village': // 小区名
                    'province': // 省份
                    'city': // 市
                    'region': // 区
                    'release_time': // 审核通过时间
                    'create_tiem': // 创建时间
                    'valid_time': // 订单到期时间
                    'process_status': // 向用户展示当前进度描述的文字
                },
                / ...
            ]
        }
    }

## （设计师）提交方案

### 场景

设计师提交设计方案

### 获取方式

    POST

### 参数

#### header

    token

#### query

    {
        'id': // 订单ID
        'solution': [
            {
                'imageID': // 图片ID
                'explain': // 设计描述
            },
            // ...
        ],
        '3Durl': // 3d案例的链接
        'explain': // 设计说明
        'processID': // 进度ID
        'type': // 初稿、二稿、终稿
    }

### 返回数据

    {
        // ... status, messages
    }

## （设计师）获取订单进度详情

### 场景

设计师查看自己单个订单的进度状态

### 获取方式

    GET

### 参数

#### header

    token

#### query

    {
        'id': // 订单ID
    }

### 返回数据

    {
        // ... status, messages
        'content': {
            'my_order_list':[
                {
                    'processID': // 进度ID
                    'stepnum':  // 进度步骤数
                    'steptitle':  // 进度步骤名称
                    'choose':  // 用户选择状态
                    'examine':  // 步骤审核状态
                }
                // ...
            ]
        }
    }

## （设计师）查看订单设计详情

### 场景

设计师查看自己提交的方案

### 获取方式

    GET

### 参数

#### header

    token

#### query

    {
        'processID': // 进度ID
    }

### 返回数据

    {
        // ... status, message
        {
            'solution': [
                {
                    'image':{
                        'small_path': String, // 图片路径
                        'medium_path': String, // 图片路径
                        'big_path': String, // 图片路径
                    }
                    'explain': // 设计描述
                },
                // ...
            ],
            '3Durl': // 3d案例的链接
            'explain': // 设计说明
        }
    }

## （用  户）查看订单设计详情

### 场景

用户个人中心查看设计师提交的方案

### 获取方式

    GET

### 参数

#### header

    token

#### query

    {
        'id': //
        'process_type': // 0：初稿；1：终稿；2：施工图
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'content': [
                {
                    'processID': //
                    'designerID': //
                    'designer_name': //
                    '3durl': //
                    'explain': //
                    'solution': [
                        {
                            'image':{
                                'small_path': String, // 图片路径
                                'medium_path': String, // 图片路径
                                'big_path': String, // 图片路径
                            }
                            'part_name': // 房间名称
                            'explain': // 设计描述
                        },
                        // ...
                    ],
                    'examine_time': //
                    'useraction':  // 用户处理情况
                }
                // ...
            ]
        }
    }

## （用  户）选择设计方案

### 场景

用户在个人中心选择喜欢的方案（初稿、终稿）

### 获取方式

    POST

### 参数

#### header

    token

#### query

    {
        'id': Int, // 订单ID
        'processID': Int, // 进度ID
        'isselected': Boolean // 选择||淘汰
        'explain': String // 理由
        'process_type': // 0：初稿；1：终稿；2：施工图
    }

### 说明

需要根据进度类型process_type判断该订单该进度是否达到了选择数量上限，达到返回message错误提示

### 返回数据

    {
        // ... status, message
    }

## （用  户）评价

### 场景

订单完成，用户对设计师评价

### 获取方式

    POST

### 参数

#### header

    token

#### query

    {
        'id': // 订单ID
        'designer_id': // 设计师ID
        'score': Int, // 分数
        'comment': String // 评价详情
    }

### 返回数据

    {
        // ... status, message
    }

## （公  司）获取订单列表

### 获取方式

    GET

### 参数

#### header

    token

#### query

    {
        'status': // 默认为空，查询全部；1查询进行中的订单；2查询已经完结的订单
    }

### 返回数据

    {
        // ... status, message
        'content':{
            task_list': [
                {
                    'id': // 订单id
                    'sn': // 订单编号
                    'village': // 小区名称
                    'area': // 面积
                    'style': // 风格
                    'unit': // 户型
                    'designer_id': // 抢单设计师
                    'designer_name': // 抢单设计师
                    'need_review': // 是否需要审核设计初稿或终稿，从进度表查询审核状态
                    'step_num': // 订单当前进度步数
                    'step_title': // 订单当前进度名称
                }
                // ...
            ]
        }
    }

### 说明

数据列表排序：最新需审核的订单排在最前面，其余按时间倒序

## （公  司）查看订单详情

### 场景

公司个人中心查看订单详情和设计师提交的方案

### 获取方式

    GET

### 参数

#### header

    token

#### query

    {
        'id': // 订单id
        'designer_id': // 设计师id
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'content': [
                {
                    'task': {
                        'sn': // 订单编号
                        'release_time': // 订单发布时间
                        'area': // 面积
                        'unit': // 户型
                        'house_status': // '旧房翻新'||'新房'
                        'hosue_type': // '跃层'||'平层'||'别墅'
                        'province': // 省
                        'city': // 市
                        'region': // 区
                        'village': // 小区名称
                        'design_budget': // 设计预算
                        'budget': // 装修预算
                        'info': // 备注内容
                        'address': // 地址
                        'measure_time': // 量房时间
                        'customer': {
                            'name': // 客户姓名
                            'hobby': // 爱好
                            'phone': // 客户电话，此字段需鉴权，用户选择初稿后才返回此字段
                            'members': [
                                {
                                    'relation': // 与用户的关系，父亲、母亲..
                                    'age': // 年龄
                                    'hobby': // 爱好
                                    'style': // 偏爱风格
                                    'color': // 偏好颜色
                                    'info': // 备注
                                }
                                // ...
                            ]
                        },
                        'step_title': // 进度状态，从进度表查询
                    },
                    'solution': { // 方案
                        'designer_id': // 设计师id
                        'designer_name': // 设计师姓名
                        'first_draft': { // 初稿
                            'process_id': //
                            '3durl': //
                            'examine_time': //
                            'content': [
                                {
                                    'image':{
                                        'small_path': String, // 图片路径
                                        'medium_path': String, // 图片路径
                                        'big_path': String, // 图片路径
                                    }
                                    'part_name': // 房间名称
                                    'explain': // 设计描述
                                },
                                // ...
                            ],
                            'explain': //
                            'useraction':  // 用户处理情况
                        },
                        'final_draft':{ // 终稿
                            // 字段初稿
                        },
                        'cad':{ // 施工图凭证
                            'process_id': //
                            'examine_time': //
                            'image':{
                                'small_path': String, // 图片路径
                                'medium_path': String, // 图片路径
                                'big_path': String, // 图片路径
                            }
                            'explain': // 施工图凭证描述
                        },
                    }
                }
                // ...
            ]
        }
    }

## （公  司）审核设计师的设计

### 获取方式

    post

### 参数

#### header

    token

#### query

    {
        'process_id': // 进度id
        'ispass': Boolean // 通过与否
        'explain': String // 处理说明
    }

### 返回数据

    {
        // ... status, message
    }

## （公  司）查看员工列表

### 参数

#### header

    token

#### query

    {
        'type': // 默认在职，1为离职；
        'role_id': // 角色id；默认为3；设计师3，工长11，工程部经理12，材料部经理13，监理14，巡检15
        'page': // 当前页
        'size': // 每页显示数量
    }

### 返回数据

    {
        // ... status, message
        'content':{
            'staff_list':[
                {
                    'name': // 设计师名字
                    'id': // 设计师id
                    'level': // 设计师等级
                    'title': // 设计师称谓；高级设计师、中级设计师、资深设计师...
                    'location': {
                        'province': // 省
                        'city': // 市
                        'region': // 区
                    }
                    'work_years': // 从业年限
                    'good_style': // 擅长风格
                    'photo':{
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }
                },
                // ...
            ],
            'page_total': // 数据总条数
        }
    }

## （公  司）更改员工称谓和在职状态

### 参数

#### header

    token

#### query

    {
        'designer_id_list': Array, // 设计师们的id
        'title': String // 设计师称谓
        'isincumbent': 1||2 // 1:在职；2：离职
    }

### 返回数据

    {
        // ... status, message
    }

## （公  司）查看工地

### 参数

#### header

    token

#### query

    {
        'process': // 默认全部 1:施工中；2：已竣工；
        'page':
        'size':
    }

### 返回数据

    {
        // ... status, message
        'content':{
            'project_list': [
                {
                    'project_id': 工地id
                    'village': // 小区名称
                    'costomer_name': // 业主姓名
                    'area': // 面积
                    'unit': // 户型
                    'style': // 风格
                    'complete_status': // 施工状态
                    'province': // 省
                    'city': // 市
                    'region': // 区
                    'address': // 地址
                    'cover':{
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }
                }
                // ...
            ],
            'page_total': // 数据总条数
        }
    }

## （公  司）删除工地

### 参数

#### header

    token

#### query

    {
        'project_id_list': Array // 工地的id们
    }

### 返回数据

    {
        // ... status, message
    }

## （平台首页）获取公司列表

### 参数

    {
        'query': {
            'keyword': // 用户查询的关键字，默认为空。
            'service_type': // 家装公司、工装公司、软装公司
            'region': // 区
            'credit_level': // 信用等级
            'ensure': // 装修保障
            'isrecommend': // 默认空；1为推荐的公司
            'isindex': // 默认为空，获取全部。1为获取index的。
        },
        'sort': String, // total_score总评分\praise口碑\design设计分\service服务分\quality工程质量分\project_score工地分\
        'page': // 页码
        'size': // 页长
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'company_list': [
                {
                    'id': // 公司ID
                    'name': // 公司名
                    'credit_level': // 信用级别
                    'case_num': // 案例数
                    'project_num': // 工地数
                    'comment_num': // 公司评论数
                    'location': {
                        'province': // 省
                        'city': // 市
                        'region': // 区
                        'address': // 地址
                        'longitude': // 经度
                        'latitude': // 纬度
                    }
                    'phone': // 公司电话
                    'consult_num': // 公司咨询量
                    'praise': // 口碑值
                    'design_score': // 设计分
                    'service_score': // 服务分
                    'quality_score': // 工程质量分
                    'project_score': // 工地分
                    'logo': { // logo图
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }，
                    'cover': { // 封面图
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }
                },
                // ...
            ],
            'page_total': // 数据总条数
        }
    }

## （公司首页）查看公司设计师列表

### 场景

用户查看公司信息，只展示在职设计师

### 参数

    {
        'id': // 公司ID
        'keyword': // 用户查询的关键字，默认为空。
        'style': // 风格id。现代、田园 ...
        'sort_by': //排序按: 1人气，2评分，3案例数，4从业年限，5收费标准
        'page': // 当前页
        'size': // 每页显示数量
    }

### 返回数据

    {
        // ... status, message
        'content':{
            'designer_list':[
                {
                    'id': // 设计师id
                    'name': // 设计师名字
                    'level': // 设计师等级
                    'title': // 设计师称谓；高级设计师、中级设计师、资深设计师...
                    'location': {
                        'province': // 省
                        'city': // 市
                        'region': // 区
                    }
                    'work_years': // 从业年限
                    'good_style': // 擅长风格
                    'photo':{ // 设计师照片
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    },
                    'case_num': // 案例数
                    'case_list': [ // 3条案例
                        {
                            'id': // 案例id
                            'name': // 案例名
                            'cover': { // 案例封面图
                                'ori_path': // 原始图路径
                                'big_path': // 大图路径
                                'mid_path': // 中图路径
                                'sml_path': // 小图路径
                            }
                        }
                        // ...
                    ]
                },
                // ...
            ],
            'page_total': // 数据总条数
        }
    }

## （平台首页）获取装修公司活动

### 场景

前端页面展示最新装修公司活动的推荐列表

### 参数

    {
        'keyword': // 用户查询的关键字，默认为空。
        'isrecommend': // 默认为空，获取全部；1为获取推荐的。
        'valid': // 默认为空，获取全部。1为获取有效期内的，2为过期的。
        'ishot': // 默认为空，获取全部。1为获取hot的。
        'isindex': // 默认为空，获取全部。1为获取index的。（平台的字段）
        'page': // 页码
        'size': // 页长
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'action_list': [
                {
                    'id': // 活动id
                    'title': // 活动标题
                    'discription': // 活动描述
                    'url': // 活动详情页
                    'cover': { // 封面图
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }
                },
                // ...
            ],
            'page_total': // 总页数
        }
    }

### 说明

- 返回数据按level倒序排序

## （平台/公司/设计师）获取在建工地列表

### 参数

    {
        'company_id': // 公司id，默认为空。
        'designer_id': // 设计师id，默认为空。
        'keyword': // 用户查询的关键字，默认为空。
        'process': // 默认全部 1:施工中；2：已竣工；
        'isrecommend': // 默认为空，获取全部；1为获取推荐的。
        'isindex': // 默认为空，获取全部。1为获取index的。
        'style': // 风格id。
        'unit': // 户型id。
        'page':
        'size':
    }

### 返回数据

    {
        // ... status, message
        'content':{
            'project_list': [
                {
                    'project_id': 工地id
                    'village': // 小区名称
                    'costomer_name': // 业主姓名
                    'designer': // 设计师
                    'foreman': // 工长
                    'supervisor': // 监理
                    'area': // 面积
                    'unit': // 户型
                    'style': // 风格
                    'cost': // 造价
                    'complete_status': // 施工状态
                    'current_step': // 当前施工进度（大阶段nodeid）
                    'location': {
                        'province': // 省
                        'city': // 市
                        'region': // 区
                        'address': // 地址
                        'longitude': // 经度
                        'latitude': // 纬度
                    }
                    'fans_num': // 关注数
                    'cover':{
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }
                }
                // ...
            ],
            'page_total': // 数据总条数
        }
    }

## （用  户）喜欢

### 场景

用户点击喜欢在建工地

### 参数

    {
        'project_id': // 工地id
    }

### 返回数据

    {
        // ... status, message
    }

## （平台首页）推荐设计师列表

### 参数

    {
        'isrecommend': // 默认为空，获取全部；1为获取推荐的。
        'isindex': // 默认为空，获取全部。1为获取index的。
        'page': // 页码
        'size': // 页长
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'designer_list': [
                {
                    'id': // 设计师id
                    'name': // 设计师名字
                    'level': // 设计师等级
                    'concept': // 设计师理念
                    'photo': { // 照片
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }
                    'case_num': // 案例数
                },
                // ...
            ],
            'page_total': // 总页数
        }
    }

## （平台/公司/设计师）获取案例列表

### 参数

    {
        'keyword': // 用户查询的关键字，默认为空。
        'company_id': // 公司id，为空查询全部
        'designer_id': // 设计师id，为空查询全部
        'isrecommend': // 默认为空，获取全部；1为获取推荐的。
        'isindex': // 默认为空，获取全部。1为获取index的。
        'style': // 风格id
        'unit': // 户型id。
        'type': // 类型id。家装、工装、软装
        'page':
        'size':
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'case_list': [
                {
                    'id': // 案例id
                    'name': // 案例name
                    'style': // 案例风格
                    'unit': // 案例户型
                    'area': // 面积
                    'cost': // 案例造价
                    'funs_num': // 赞数
                    'cover': { // 封面图
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    },
                    'designer':{ // 设计师
                        'id': //
                        'name': //
                        'avatar': { // 照片
                            'ori_path': // 原始图路径
                            'big_path': // 大图路径
                            'mid_path': // 中图路径
                            'sml_path': // 小图路径
                        },
                    }
                }
                // ...
            ],
            'page_total': //
        }
    }

## （公司首页）获取装修公司详情

### 参数

    {
        'company_id': // 装修公司id
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'content': {
                'id': // 装修公司id
                'name': // 装修公司名字
                'phone': // 电话
                'credit_level': // 装修公司信用等级
                'total_score': // 总评分
                'praise': // 装修公司口碑
                'design': // 装修公司设计分
                'service': // 装修公司服务分
                'quality': // 装修公司工程质量分
                'project': // 装修公司工地分
                'back_image': // 装修公司背景图路径
                'logo': { // logo图
                    'ori_path': // 原始图路径
                    'big_path': // 大图路径
                    'mid_path': // 中图路径
                    'sml_path': // 小图路径
                },
                'location': {
                    'province': // 省
                    'city': // 市
                    'region': // 区
                    'address': // 地址
                },
                'case_num': // 案例数
                'fans_num': // 人气数
                'introduce': // 公司简介
                'license': [ // 资质证书
                    {
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }
                    // ...
                ],
                'service_content': {
                    'type_list': Array, // id=>名称
                    'region_list': Array, // id=>名称
                    'price': String,
                    'style': Array, // id=>名称
                }
            }
        }
    }

## （公司首页）获取装修公司活动列表

### 场景

前端页面展示装修公司最新活动的推荐列表

### 参数

    {
        'company_id': // 装修公司id。
        'keyword': // 用户查询的关键字，默认为空。
        'isrecommend': // 默认为空，获取全部；1为获取推荐的。
        'valid': // 默认为空，获取全部。1为获取有效期内的，2为过期的。
        'ishot': // 默认为空，获取全部。1为获取hot的。
        'isindex': // 默认为空，获取全部。1为获取index的。（公司的字段）
        'page': // 页码
        'size': // 页长
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'action_list': [
                {
                    'id': // 活动id
                    'title': // 活动标题
                    'discription': // 活动描述
                    'url': // 活动详情页
                    'starttime': // 开始时间
                    'endtime': // 结束时间
                    'cover': { // 封面图
                        'ori_path': // 原始图路径
                        'big_path': // 大图路径
                        'mid_path': // 中图路径
                        'sml_path': // 小图路径
                    }
                },
                // ...
            ],
            'page_total': // 总页数
        }
    }

### 说明

- 返回数据按level倒序排序

## （公司页面）获取设计师详情

### 参数

    {
        'id': // 设计师id
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'content': {
                'id': // 设计师id
                'name': // 设计师名字
                'phone': // 设计师电话
                'level': // 设计师等级
                'title': // 设计师称谓；高级设计师、中级设计师、资深设计师...
                'location': {
                    'province': // 省
                    'city': // 市
                    'region': // 区
                }
                'score': // 评分
                'case_num': // 案例数
                'fans_num': // 人气数
                'work_years': // 从业年限
                'education': // 学历
                'school': // 学校
                'field': // 专业
                'good_part': // 擅长空间
                'good_style': // 擅长风格
                'company': // 所属公司
                'photo':{ // 设计师照片
                    'ori_path': // 原始图路径
                    'big_path': // 大图路径
                    'mid_path': // 中图路径
                    'sml_path': // 小图路径
                },
                'praise_list': [ // 个人荣誉
                    String
                    // ...
                ]
            }
        }
    }

## （公司页面）获取公司评价

### 参数

    {
        'id': // 公司id
        'query': // 1为工地，2为设计，3为案例，默认为1
    }

### 返回数据

    {
        // ... status, message
        'content': {
            'comment_list': [
                {
                    'user': { // 评论人
                        'avatar': { // 头像
                            'ori_path': // 原始图路径
                            'big_path': // 大图路径
                            'mid_path': // 中图路径
                            'sml_path': // 小图路径
                        },
                        'nick': // 昵称
                    },
                    'time': // 评论时间
                    'content': // 评论内容
                },
                // ...
            ]
        }
    }

## 非业主评论（工地、设计、案例）

### 参数

#### header

    token

#### query

    {
        'type': // 1工地，2设计，3案例
        'id': // 工地id、设计id、案例id
        'content': String // 评论内容
    }

### 返回数据

    {
        // ... status, message
    }