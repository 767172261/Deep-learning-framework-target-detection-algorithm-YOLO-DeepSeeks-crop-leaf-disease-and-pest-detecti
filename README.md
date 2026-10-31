深度学习框架目标检测算法YOLO+DeepSeek的农作物叶片病虫害检测系统 使用YOLOV1-YOLOV12任意模型进行目标检测
@[toc]
以下文字及代码仅供参考学习使用。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ceb24057bede45bb8cbcfde48490ec32.png)
基于Vue3和Element Plus的登录界面，创建名为`Login.vue`的组件。这个组件将包含用户名和密码输入框、登录按钮以及注册账号和忘记密码的链接。

### 1. 创建 `Login.vue`

在你的项目中创建一个新的文件 `src/views/Login.vue` 并添加以下代码：

```vue
<template>
  <div class="login-container">
    <el-card class="login-card">
      <h2>基于YOLO+DeepSeek的农作物病虫害检测系统</h2>
      <p>Crop Disease and Insect Pests Detection System Based on YOLO+DeepSeek</p>

      <el-form :model="loginForm" status-icon :rules="rules" ref="loginFormRef" label-width="0px" class="demo-ruleForm">
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" placeholder="请输入用户名" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input type="password" v-model="loginForm.password" autocomplete="off" placeholder="请输入密码" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" style="width: 100%" @click="submitForm('loginFormRef')">登录</el-button>
        </el-form-item>
      </el-form>

      <div class="bottom-links">
        <span @click="navigateToRegister">注册账号</span> |
        <span @click="navigateToForgotPassword">忘记密码</span>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          // 这里可以调用后端API进行登录验证
          console.log('submit!');
          this.login();
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    login() {
      // 登录逻辑，例如调用后端API进行用户认证
      console.log('尝试登录:', this.loginForm);
      // 成功后跳转到首页或其他页面
      this.$router.push('/home');
    },
    navigateToRegister() {
      // 跳转到注册页面
      this.$router.push('/register');
    },
    navigateToForgotPassword() {
      // 跳转到忘记密码页面
      this.$router.push('/forgot-password');
    }
  }
};
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #8BC34A; /* 绿色背景 */
}

.login-card {
  width: 350px;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background-color: white;
}

.bottom-links {
  text-align: center;
  margin-top: 10px;
}
</style>
```

### 2. 配置路由

确保你的路由配置支持导航到登录页面。在 `src/router/index.js` 中添加如下配置：

```javascript
import Vue from 'vue';
import Router from 'vue-router';
import Login from '@/views/Login.vue';

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/',
      redirect: '/login'
    },
    {
      path: '/login',
      name: 'Login',
      component: Login
    },
    // 其他路由...
  ]
});
```

### 3. 后端登录接口

在后端（如Flask）中实现登录接口，用于验证用户名和密码，并返回相应的响应。

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/api/login', methods=['POST'])
def login():
    data = request.json
    username = data.get('username')
    password = data.get('password')

    # 这里应该连接数据库进行验证
    if username == 'admin' and password == 'password':
        return jsonify({'success': True, 'message': '登录成功'})
    else:
        return jsonify({'success': False, 'message': '用户名或密码错误'})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### 4. 前端调用后端登录接口

在 `Login.vue` 的 `login` 方法中，使用 `axios` 发送请求到后端登录接口。

```javascript
methods: {
  async login() {
    try {
      const response = await axios.post('/api/login', this.loginForm);
      if (response.data.success) {
        console.log('登录成功');
        this.$router.push('/home');
      } else {
        console.error('登录失败:', response.data.message);
      }
    } catch (error) {
      console.error('登录请求失败:', error);
    }
  }
}
```

可进一步优化界面样式和功能。
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/db5bb15fa46a41938c75ec48e3a74287.png)
1
![1](https://i-blog.csdnimg.cn/direct/9cf10f26b92b4d229ff4faec21916e7c.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/57c973087d2f41babd2572019a130305.png)
构建完整的农作物病虫害检测系统，包括前端、后端、数据库和模型服务等多个部分。以下是详细的代码示例，帮助你实现这个系统的各个功能模块。
仅供参考学习使用。

### 1. 前端（Vue3 + Element Plus + Echarts）

#### 安装依赖

```bash
npm install vue@next element-plus echarts axios
```

#### 创建 Vue 组件

在 `src/components` 目录下创建 `Home.vue` 文件：

```vue
<template>
  <div class="home">
    <!-- 左侧菜单 -->
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
      <el-menu :default-openeds="['1', '3']">
        <el-submenu index="1">
          <template #title><i class="el-icon-message"></i>导航一</template>
          <el-menu-item-group>
            <el-menu-item index="1-1">选项1</el-menu-item>
            <el-menu-item index="1-2">选项2</el-menu-item>
          </el-menu-item-group>
        </el-submenu>
        <el-submenu index="2">
          <template #title><i class="el-icon-menu"></i>导航二</template>
          <el-menu-item-group>
            <el-menu-item index="2-1">选项1</el-menu-item>
            <el-menu-item index="2-2">选项2</el-menu-item>
          </el-menu-item-group>
        </el-submenu>
        <el-submenu index="3">
          <template #title><i class="el-icon-setting"></i>导航三</template>
          <el-menu-item-group>
            <el-menu-item index="3-1">选项1</el-menu-item>
            <el-menu-item index="3-2">选项2</el-menu-item>
          </el-menu-item-group>
        </el-submenu>
      </el-menu>
    </el-aside>

    <!-- 主要内容区域 -->
    <el-container>
      <el-header style="text-align: right; font-size: 12px">
        <el-dropdown>
          <i class="el-icon-setting" style="margin-right: 15px"></i>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>查看</el-dropdown-item>
            <el-dropdown-item>新增</el-dropdown-item>
            <el-dropdown-item>删除</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <span>linfeng</span>
      </el-header>

      <el-main>
        <!-- 图表展示 -->
        <div id="chart1" style="width: 600px;height:400px;"></div>
        <div id="chart2" style="width: 600px;height:400px;"></div>
        <div id="chart3" style="width: 600px;height:400px;"></div>
        <div id="chart4" style="width: 600px;height:400px;"></div>
        
        <!-- 实时预测信息表格 -->
        <el-table :data="realtimePredictions" style="width: 100%">
          <el-table-column prop="username" label="用户名" width="180"></el-table-column>
          <el-table-column prop="model" label="识别权重" width="180"></el-table-column>
          <el-table-column prop="threshold" label="最小阈值" width="180"></el-table-column>
          <el-table-column prop="aiHelper" label="AI助手" width="180"></el-table-column>
          <el-table-column prop="time" label="时间"></el-table-column>
        </el-table>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  data() {
    return {
      realtimePredictions: [
        { username: 'admin', model: 'best.onnx', threshold: 0.65, aiHelper: 'Qwen', time: '2025-04-24 10:35:30' },
        // 其他数据...
      ]
    };
  },
  mounted() {
    this.initCharts();
  },
  methods: {
    initCharts() {
      // 初始化图表1
      const chart1 = echarts.init(document.getElementById('chart1'));
      const option1 = {
        // 配置项...
      };
      chart1.setOption(option1);

      // 初始化图表2
      const chart2 = echarts.init(document.getElementById('chart2'));
      const option2 = {
        // 配置项...
      };
      chart2.setOption(option2);

      // 初始化图表3
      const chart3 = echarts.init(document.getElementById('chart3'));
      const option3 = {
        // 配置项...
      };
      chart3.setOption(option3);

      // 初始化图表4
      const chart4 = echarts.init(document.getElementById('chart4'));
      const option4 = {
        // 配置项...
      };
      chart4.setOption(option4);
    }
  }
};
</script>

<style scoped>
.home {
  display: flex;
}
.el-aside {
  background-color: #D3DCE6;
  color: #333;
  text-align: center;
  line-height: 200px;
}

.el-main {
  background-color: #E9EEF3;
  color: #333;
  text-align: center;
  line-height: 160px;
}

body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
</style>
```

### 2. 后端（Flask）

#### 安装依赖

```bash
pip install flask ultralytics opencv-python numpy mysql-connector-python
```

#### 创建 Flask 应用

在项目根目录下创建 `app.py` 文件：

```python
from flask import Flask, request, jsonify, send_file
from ultralytics import YOLO
import cv2
import numpy as np
import io
from PIL import Image
import mysql.connector

app = Flask(__name__)
model = YOLO('best.pt')  # 加载YOLO模型

# 连接MySQL数据库
db = mysql.connector.connect(
    host="localhost",
    user="yourusername",
    password="yourpassword",
    database="yourdatabase"
)

@app.route('/api/detect', methods=['POST'])
def detect():
    file = request.files['image']
    img_bytes = file.read()
    img_array = np.frombuffer(img_bytes, np.uint8)
    img = cv2.imdecode(img_array, cv2.IMREAD_COLOR)
    results = model(img)  # 执行检测
    
    output = {'detections': []}
    for box in results[0].boxes:
        label = model.names[int(box.cls)]
        confidence = float(box.conf)
        x1, y1, x2, y2 = [float(coord) for coord in box.xyxy]
        output['detections'].append({'label': label, 'confidence': confidence, 'bbox': [x1, y1, x2, y2]})
    
    # 将检测结果图像保存为内存中的文件
    result_img = results[0].plot()
    img_pil = Image.fromarray(result_img[..., ::-1])
    img_io = io.BytesIO()
    img_pil.save(img_io, 'JPEG')
    img_io.seek(0)
    
    return jsonify({
        'resultImage': f'data:image/jpeg;base64,{base64.b64encode(img_io.getvalue()).decode()}',
        'detectionResults': output['detections'],
    })

@app.route('/api/realtime_predictions', methods=['GET'])
def get_realtime_predictions():
    cursor = db.cursor()
    cursor.execute("SELECT * FROM realtime_predictions")
    rows = cursor.fetchall()
    predictions = []
    for row in rows:
        predictions.append({
            'username': row[0],
            'model': row[1],
            'threshold': row[2],
            'aiHelper': row[3],
            'time': row[4]
        })
    return jsonify(predictions)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### 3. 数据库设计

创建一个名为 `realtime_predictions` 的表来存储实时预测信息：

```sql
CREATE TABLE realtime_predictions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255),
    model VARCHAR(255),
    threshold FLOAT,
    aiHelper VARCHAR(255),
    time DATETIME
);
```

### 4. AI建议模块

 DeepSeek 和 Qwen 提供了 API 接口，可以通过以下方式调用它们：

```python
import requests

def get_advice_from_deepseek(detection):
    response = requests.post("http://deepseek/api/advice", json=detection)
    return response.json()

def get_advice_from_qwen(detection):
    response = requests.post("http://qwen/api/advice", json=detection)
    return response.json()
```

### 5. 用户管理和权限控制（SpringBoot + MyBatis-Plus）

这部分涉及到用户注册、登录、角色管理等功能，可以使用 Spring Security 与 MyBatis-Plus 结合实现。具体的实现会依赖于你的具体需求和数据模型设计。

### 6. 数据可视化（Echarts）

在 Vue3 项目中集成 Echarts 进行数据可视化，可以展示病虫害的分布情况等信息。这通常包括创建一个 Vue 组件，并在其中初始化 Echarts 实例。

### 7. 导出 PDF 报告
使用 Python 的 `fpdf` 或 JavaScript 的 `jsPDF` 库来生成包含检测结果和建议的 PDF 文件。


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c8997f500c9b40fdacf36eee2b626c6d.png)

