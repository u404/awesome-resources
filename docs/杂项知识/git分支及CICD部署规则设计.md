### 模式介绍：
该分支管理模式，充分参考了阿里的AoneFlow 分支管理模式，并根据目前公司规模特点进行简化：  
- master分支——对应阿里的master分支  
- develop分支——兼有阿里的特性分支和release分支的功能。注：阿里的流水线特性分支模式中 release分支是自动生成的，专门用于合并多个特性分支，并部署迭代。  
- local分支——类似于特性分支

### 模式特点：
- 基于迭代，创建动态分支。支持多迭代并存，互不干扰。
- 单环境可被多个分支复用，单分支可部署到多个环境，分支与环境完全解耦。
- 支持：多迭代分别测试，独立发布；多迭代共同测试，独立发布；多迭代公共测试，共同发布。
- 简而言之，在迭代未上线之前，所有迭代都是持续独立的，在必要时可分可合。

### 参考文档：
- https://blog.csdn.net/charry0110/article/details/117753237  
- https://help.aliyun.com/document_detail/67119.html?spm=a262eq.13248089.0.0  
- https://www.cnblogs.com/tomakemyself/p/13829985.html  

### 详细方案
1. 分支主要分为：
   - master - 代码归档分支，不可直接提交代码，只能由develop（feature/hotfix）或release（暂不需要）合并，合并时必须打tag（Release_x.x.x），合并权限由仓库管理者或审核者进行。
   - develop（feature/hotfix）类 - 动态建立的分支，根据每次需求迭代创建，如：feature/v1.1.0。删减了release的模式下，兼具了release的一些作用。
   - local 类 - 个人自行创建管理，视需要是否上传到远程仓库，自测完成后，合并到develop（feature/hotfix）。
2. 版本迭代分为两类：
   - 一类是功能迭代Feature类（命名参考：feature/v1.1.0），feature类分支主要更新主版本号或子版本号，少数情况更新修订版本号。
   - 一类是bug紧急修复或更新Hotfix（命名参考：hotfix/v1.1.1），hotfix类分支只更新修订版本号。
3. 分支与运维环境CI/CD的关联设置：
   - feature/ 和 hotfix/ 下的分支，默认关联 dev环境CI/CD，所有feature/ 和 hotfix/  和 release/下的分支，推送代码即自动部署到dev环境。
   - feature/ 和 hotfix/ 下的分支，如果想部署到 test 环境，需要在git操作里手动点击操作，选择该分支部署到test环境。这个操作者，可以考虑交给测试人员控制。
   - feature/ 和 hotfix/ 下的分支，如果想部署到 pre 环境，只能由测试人员控制手动操作，选择该分支部署到pre环境。
   - 线上环境，只能由 feature/ 或 hotfix/ 下分支，只能由主程或指定人员合并到master分支，并打上tag，再由运维发布线上。
4. 功能迭代开发：
   - 从 master 上拉取 feature类分支（以feature/v1.1.0为例）——开始开发迭代周期。
   - 个人开发时，自己从feature/v1.1.0创建local类分支，以姓名为前缀，其后的名字自行管理区分（如：zhangsan/v1.1.0_musicServiceUpdate），自行视情况是否上传到远程仓库中，一次迭代中，可以针对多个模块或微服务分别创建多个local类分支（如针对订单服务可以在建一个zhangsan/v1.1.0_orderServiceUpdate）。
   - 当某一个local类分支开发完成后，本地自测通过后，合并到feature/v1.1.0，本步骤中可能会需要解决冲突，与相关人员协同处理。合并推送后，dev环境将会自动部署。
   - 若dev环境，进一步自测没有问题，则可以通过邮件通知测试人员提测，将分支feature/v1.1.0告诉告诉测试人员，由测试人员协调之后，自行选择时间将该分支部署到test环境。——进入测试流程。
   - 若test环境发现bug，由测试提bug，开发人员重复进行c - d 的过程，提交修复。若test环境测试通过，由测试人员自行选择时间，将该分支部署到pre环境；若测试环境中，测试人员觉得测试困难，可以由口头或邮件将该提测打回。可将test环境重新还原为其他待测试的feature/hotfix分支或master分支。
   - 若pre环境测试通过，测试人员可以通知相应的主程或仓库管理者，合并到master并打一个tag。在相应的发布时间，通知运维上线。
   - 发布线上，需要测试或产品或运营等相关同学，进一步校验，如果有问题，及时进行hotfix。如果该feature或hotfix会影响正在进行的feature（如：feature/v1.2.0），那么feature/v1.2.0 分支及时同步 master 分支。——一次开发迭代过程完毕。
5. 两个迭代有重叠时间的并行测试情况：
   - 轻量级、模块化、微前端、微服务的模式中，在某些场景下，依然会出现两个迭代有时间重叠的情况。如：bug hotfix 与 feature 重叠、两个或更多的并行迭代依赖的同一底层服务在多个feature中都有更改。
     - 一种方案是，错开测试时间。像hotfix的测试，可以测试环境中先替换掉feature，优先测试，紧急发布。
     - 另一种方案是，并行测试，独立发布。同一个服务在两个feature中都有更改的场景，根据feature版本的先后顺序（如：feature/v1.1.2 和 feature/v1.1.3），以版本号顺序建立一个 feature/v1.1.3_1.1.2的合并分支（可能会涉及到冲突解决），由测试人员部署测试环境，并行测试两个feature。若feature/v1.1.2的相关功能测试完成，则可以独立发布。feature/v1.1.2 发布完成后，及时从master合并到feature/v1.1.3中（可能需要再解决一次冲突，如果不想再解决，也可以将feature/v1.1.3_1.1.2合并回feature/1.1.3，但这种反向合并，不适用于3个分支及以上并行测试的情况）

