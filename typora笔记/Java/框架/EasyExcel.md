[EasyExcel(xlsx表格，1谁1是哦)](https://easyexcel.opensource.alibaba.com/)

​    EasyExcel是阿里巴巴开源的一个excel处理框架，**以使用简单、节省内存著称**。EasyExcel能大大减少占用内存的主要原因是在解析Excel时没有将文件数据一次性全部加载到内存中，而是从磁盘上一行行读取数据，逐个解析。

   EasyExcel是一个开源的Java操作Excel文件的工具，可以使用它来读取、写入和操作Excel文件。

## EasyExcel特点：

- Java领域解析、生成Excel比较有名的框架有Apache poi、jxl等。但他们都存在一个严重的问题就是非常的耗内存。如果你的系统并发量不大的话可能还行，但是一旦并发上来后一定会OOM或者JVM频繁的full gc。
- **EasyExcel采用一行一行的解析模式**，并将一行的解析结果以观察者的模式通知处理（AnalysisEventListener）
- EasyExcel是一个基于Java的简单、省内存的读写Excel的开源项目。在尽可能节约内存的情况下支持读写百M的Excel。


特点：

![](../../../%25E7%25AC%2594%25E8%25AE%25B0%25E5%259B%25BE%25E7%2589%2587/Java/%25E6%25A1%2586%25E6%259E%25B6/EasyExcel/EasyExcel%25E7%2589%25B9%25E7%2582%25B9.png)





## 基本使用：

读写操作流程：

![](../../../%25E7%25AC%2594%25E8%25AE%25B0%25E5%259B%25BE%25E7%2589%2587/Java/%25E6%25A1%2586%25E6%259E%25B6/EasyExcel/32-EasyExcel%25E8%25AF%25BB%25E5%2586%2599%25E6%2593%258D%25E4%25BD%259C.png)

### 依赖：

```xml
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>easyexcel</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

### 写操作：

#### 实体类：

```java
import com.alibaba.excel.annotation.ExcelProperty;
import lombok.Data;

/*Excel表格-写操作*/
@Data //
public class User {

    @ExcelProperty("学生id") // TODO Excel表格设置表头名称
    private int id;

    @ExcelProperty("用户名称")
    private String name;
}
```

#### ExcelWrite

```java
import com.alibaba.excel.EasyExcel;

import java.util.ArrayList;
import java.util.List;

/* Excel表格-写操作 */
public class ExcelWrite {
    public static void main(String[] args) {
        // 文件路径和名称
        String fileName = "G:\\Excel表格.xlsx";
        // 调用方法   write(OutputStream outputStream):流方式
        //        write(String pathName, Class head)：文件名称路径
        //         ExcelWriterBuilder write(File file, Class head) :文件
        EasyExcel.write(fileName, User.class) // write(fileName,User):创建完表格了
            .sheet("写操作") //创建表格的每一页(sheet)
            .doWrite(data()); // 写数据
    }

    // for遍历写10条数据
    private static List<User> data() {
        List<User> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            User user = new User();
            user.setId(i);
            user.setName("lucy" + i);
            list.add(user);
        }
        return list;
    }
}
```

结果：

![](../../../%25E7%25AC%2594%25E8%25AE%25B0%25E5%259B%25BE%25E7%2589%2587/Java/%25E6%25A1%2586%25E6%259E%25B6/EasyExcel/%25E5%2586%2599%25E6%2593%258D%25E4%25BD%259C%25E7%25BB%2593%25E6%259E%259C.png)



### 读操作：

#### pojo(实体类):

```java
import com.alibaba.excel.annotation.ExcelProperty;
import lombok.Data;

/*Excel表格-读写操作*/
@Data //
public class User {

    // @ExcelProperty:Excel表格   value:设置表头名称,   index:从第几列读取Excel表格 
    @ExcelProperty(value = "学生id",index = 0)
    private int id;

    @ExcelProperty(value = "用户名称",index = 1)
    private String name;
}
```

#### 监听器：

```java
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;

import java.util.Map;

/**
 * Excel表格-读操作(监听器)
 */
public class ExcelListener extends AnalysisEventListener<User> {

    //    一行一行读取excel表格的内容，把每行的内容封装到user对象中
    //    注意：从excel表格的第二行开始读取。   第一行默认为表头。
    @Override
    public void invoke(User user, AnalysisContext analysisContext) {
        System.out.println("Excel内容: "+user);
    }

    // 读取表头内容
    @Override
    public void invokeHeadMap(Map<Integer, String> headMap, AnalysisContext context) {
        System.out.println("Excel表头：" + headMap);
    }

    // 读取完成后执行
    @Override
    public void doAfterAllAnalysed(AnalysisContext analysisContext) {
    }
}
```

#### 调用读监听器:

```java
import com.alibaba.excel.EasyExcel;

/* Excel表格-读操作_调用读监听器 */
public class ExcelRead {
    public static void main(String[] args) {
        // 文件路径和名称
        String fileName = "G:\\Excel表格.xlsx";
        /**
         * 调用读取方法:
         * fileName：文件路径及名称，
         * Readable：读取的实体类
         * ExcelListener: 监听器
         */
        EasyExcel.read(fileName,User.class,new ExcelListener()).sheet().doRead();
    }
}
```

结果：

![](../../../%25E7%25AC%2594%25E8%25AE%25B0%25E5%259B%25BE%25E7%2589%2587/Java/%25E6%25A1%2586%25E6%259E%25B6/EasyExcel/%25E8%25AF%25BB%25E6%2593%258D%25E4%25BD%259C%25E7%25BB%2593%25E6%259E%259C.png)



## SpringBoot整合：



### 导入/导出实体类：

   因导入、导出实体类一致，故放这。

#### Subject:

```java
@Data
@ApiModel(description = "Subject")
@TableName("subject")
public class Subject {

    private static final long serialVersionUID = 1L;
    @ApiModelProperty(value = "id")
    private Long id;

    @ApiModelProperty(value = "创建时间")
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    @TableField("create_time")
    private Date createTime;

    @ApiModelProperty(value = "更新时间")
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    @TableField("update_time")
    private Date updateTime;

    @ApiModelProperty(value = "逻辑删除(1:已删除，0:未删除)")
    @JsonIgnore
    @TableLogic
    @TableField("is_deleted")
    private Integer isDeleted;


    @ApiModelProperty(value = "类别名称")
    @TableField("title")
    private String title;

    @ApiModelProperty(value = "父ID")
    @TableField("parent_id")
    private Long parentId;

    @ApiModelProperty(value = "排序字段")
    @TableField("sort")
    private Integer sort;

    @ApiModelProperty(value = "其他参数")
    @TableField(exist = false)
    private Map<String, Object> param = new HashMap<>();

    @ApiModelProperty(value = "是否包含子节点")
    @TableField(exist = false) // 设置数据库不存在此字段
    private boolean hasChildren;

}
```

#### SubjectEeVo:

```java
@Data
public class SubjectEeVo {

    // @ExcelProperty:Excel表格   value:设置表头名称,   index:从第几列读取Excel表格
    @ExcelProperty(value = "id" ,index = 0)
    private Long id;

    @ExcelProperty(value = "课程分类名称" ,index = 1)
    private String title;

    @ExcelProperty(value = "上级id" ,index = 2)
    private Long parentId;

    @ExcelProperty(value = "排序" ,index = 3)
    private Integer sort;

}
```



### 导出：

#### 后端：

##### Service:

1. Service接口：

```java
import com.qsl.ggktparent.model.vod.Subject;
import com.baomidou.mybatisplus.extension.service.IService;

import javax.servlet.http.HttpServletResponse;
import java.util.List;

/*课程科目 服务类*/
public interface SubjectService extends IService<Subject> {

    // 导出分类课程
    void exportData(HttpServletResponse response);
}
```



2. Service实现类

```java
import com.alibaba.excel.EasyExcel;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.qsl.ggktparent.exception.BusinessException;
import com.qsl.ggktparent.model.vod.Subject;
import com.qsl.ggktparent.vo.vod.SubjectEeVo;
import com.qsl.ggktparent.vod.mapper.SubjectMapper;
import com.qsl.ggktparent.vod.service.SubjectService;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.beans.BeanUtils;
import org.springframework.stereotype.Service;

import javax.servlet.http.HttpServletResponse;
import java.net.URLEncoder;
import java.util.ArrayList;
import java.util.List;

/* 课程科目  */
@Service
public class SubjectServiceImpl extends ServiceImpl<SubjectMapper, Subject> implements SubjectService {

    // 导出分类课程
    @Override
    public void exportData(HttpServletResponse response) {
        try {
            response.setContentType("application/vnd.ms-excel"); //设置下载文件类型mime类型
            response.setCharacterEncoding("utf-8"); //防止中文乱码
            String fileName = URLEncoder.encode("课程分类文件", "UTF-8");//设置文件名称
            // 设置响应头
            response.setHeader("Content-disposition","attachment;filename="+fileName+".xlsx");
            List<Subject> subjectList = baseMapper.selectList(null); //查询所有数据

            //           List<Subject> ==转==》  List<SubjectEeVo>
            List<SubjectEeVo> subjectEeVoList= new ArrayList<>(subjectList.size());
            for (Subject subject : subjectList) {
                SubjectEeVo subjectEeVo = new SubjectEeVo();
                //    subject对象复制到subjectEeVo对象，一个个set也行。 注意：只复制相同名称的。
                BeanUtils.copyProperties(subject,subjectEeVo);
                subjectEeVoList.add(subjectEeVo);
            }

            // EasyExcel写操作
            EasyExcel.write(response.getOutputStream(),SubjectEeVo.class)
                .sheet("课程分类sheel") // 设置sheel名称
                .doWrite(subjectEeVoList);

        } catch (Exception e) {
            // 抛出自定义异常
            throw new BusinessException(20001,"导入失败");
        }
    }   
}
```

##### Controller:

```java
import javax.servlet.http.HttpServletResponse;

@Api(tags = "课程接口")
@RestController
@RequestMapping(value = "/admin/vod/subject")
public class SubjectController {

    @Autowired
    private SubjectService subjectService;

    @ApiOperation("导出分类课程")
    @GetMapping("exportData")
    public void exportData(HttpServletResponse response) {
        subjectService.exportData(response);
    }   
}
```



#### 前端：

html:

```html
<div class="app-container">
    <div class="el-toolbar">
        <div class="el-toolbar-body" style="justify-content: flex-start; margin-bottom: 5px;">
            <el-button type="primary" size="mini" @click="exportData"><i class="fa fa-plus"/> 导出</el-button>
        </div>
    </div>
```

方法：

```js
// 导出
exportData() {
  window.open('http://localhost:8301/admin/vod/subject/exportData')
},
```



### 导入：

#### 后端：

##### 监听器：

listener/SubjectListener:

```java
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;
import com.qsl.ggktparent.model.vod.Subject;
import com.qsl.ggktparent.vo.vod.SubjectEeVo;
import com.qsl.ggktparent.vod.mapper.SubjectMapper;
import org.springframework.beans.BeanUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

/* Excel表格-读取(监听器)-导入课程 */
@Component // 配置类
public class SubjectListener extends AnalysisEventListener<SubjectEeVo> {

    @Autowired
    private SubjectMapper subjectMapper;

    // 读取Excel表格数据
    @Override
    public void invoke(SubjectEeVo subjectEeVo, AnalysisContext analysisContext) {
        Subject subject = new Subject();
        // 相同的subjectEeVo复制到subject对象里
        BeanUtils.copyProperties(subjectEeVo,subject);
        subjectMapper.insert(subject);
    }

    // 读取完成后执行
    @Override
    public void doAfterAllAnalysed(AnalysisContext analysisContext) {
    }
}
```



##### Service：

接口：

```java
public interface SubjectService extends IService<Subject> {
    //     导入课程
    void importDictData(MultipartFile file);
}
```





实现类：

```java
@Service
public class SubjectServiceImpl extends ServiceImpl<SubjectMapper, Subject> implements SubjectService {

    @Autowired // 注入Excel表格监听器
    private SubjectListener subjectListener;

    //    导入课程
    @Override
    public void importDictData(MultipartFile file) {
        try {
       // EasyExcel 注意：此处不能通过new subjectListener监听器。
            EasyExcel.read(file.getInputStream(), SubjectEeVo.class, subjectListener).sheet().doRead();
        } catch (IOException e) {
            throw new BusinessException(20001, "导入失败");
        }
    }
}
```



##### Controller：

```java
@Api(tags = "课程接口")
@CrossOrigin // 支持跨域
@RestController
@RequestMapping(value = "/admin/vod/subject")
public class SubjectController {
    @Autowired
    private SubjectService subjectService;

    @ApiOperation("导入课程")
    @PostMapping("importData")
    public void importData(MultipartFile file) {
        subjectService.importDictData(file);
    }
}
```



#### 前端：

html:

```html
<div class="app-container">
    <div class="el-toolbar">
        <div class="el-toolbar-body" style="justify-content: flex-start; margin-bottom: 5px;">
            <el-button type="success" size="mini" @click="importData"><i class="fa fa-plus"/> 导入</el-button>
        </div>
    </div>


    <!-- 导入模态框   -->
    <el-dialog title="导入" :visible.sync="dialogImportVisible" width="480px">
        <el-form label-position="right" label-width="170px">
            <el-form-item label="文件">
                <el-upload
                           :multiple="false"
                           :on-success="onUploadSuccess"
                           :action="'http://localhost:8301/admin/vod/subject/importData'"
                           class="upload-demo"
                           >
                    <el-button size="small" type="primary">点击上传</el-button>
                    <div slot="tip" class="el-upload__tip">只能上传xls文件，且不超过500kb</div>
                </el-upload>
            </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="dialogImportVisible = false">取消</el-button>
        </div>
    </el-dialog>
```



js:

```js
data() {
    return {
        dialogImportVisible: false // 弹出导入模态框
    }
},  
methods: {
        // 导入
    importData() {
            this.dialogImportVisible = true
        },
            // 导入-成功弹窗
    onUploadSuccess(response, file) {
                this.$message.info('上传成功')
                this.dialogImportVisible = false // 关闭导入模态框
                this.getSubList(0) // 刷新表格
            }
    }
```



