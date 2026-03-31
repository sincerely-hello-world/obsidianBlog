
在 **pytree ROS Viewer**（即 py_trees_ros_viewer 或之前的 rqt_py_trees）中，节点颜色的标准含义如下：

- **绿色 (Green)**：节点返回 **SUCCESS**（成功）
- **红色 (Red)**：节点返回 **FAILURE**（失败）
- **蓝色 (Blue)**：节点返回 **RUNNING**（正在运行）
- **灰色 (Grey)**：未被访问过的节点（unvisited）



| 节点类型         | 执行方向  | **条件继续下一个子节点**        | 整体返回 SUCCESS 的条件           | 整体返回 FAILURE 的条件       | 典型用途            |
| ------------ | ----- | ----------------- | -------------------------- | ---------------------- | --------------- |
| **Sequence** | 左 → 右 | **子节点返回 SUCCESS** | 所有子节点都 SUCCESS             | 任意一个 FAILURE 或 RUNNING | 必须按步骤全部完成       |
| **Selector** | 左 → 右 | **子节点返回 FAILURE** | 任意一个子节点 SUCCESS（或 RUNNING） | 所有子节点都 FAILURE         | 优先级选择 / 抢占 / 后备 |

## 顺序
依次尝试，前一个返回success，才继续下一个节点。

## 选择

依次尝试， 一个running/success，返回上一级；
依次尝试， 所有都失败了，才返回失败；

## 平行