# hel-utils 业务流程说明

## 概述
本文档描述了 `hel-utils` 模块的主要业务流程，包括 Shadow DOM 节点获取流程和平台实例创建流程。

## 业务流程图说明

```mermaid
graph TB
    subgraph "hel-utils 业务流程"
        direction TB
        
        %% Shadow DOM 节点获取流程
        subgraph "Shadow DOM 节点获取流程"
            direction LR
            callGetNode["调用 getMayStaticShadowNode\n(传入名称和选项)"]
            getCustomData["getCustomData\n(从hel-micro-core获取自定义数据)"]
            checkNode["检查是否获取到节点"]
            returnShadowNode["返回Shadow DOM节点"]
            returnFallback["返回兜底节点"]
            returnBody["返回document.body"]
            
            callGetNode --> getCustomData
            getCustomData --> checkNode
            checkNode -- "找到节点" --> returnShadowNode
            checkNode -- "未找到节点" --> returnFallback
            returnFallback -- "有兜底节点" --> returnFallback
            returnFallback -- "无兜底节点" --> returnBody
        end
        
        %% 平台实例创建流程
        subgraph "平台实例创建流程"
            direction LR
            callCreateInstance["调用 createInstance\n(传入平台参数)"]
            injectPlatToMod["injectPlatToMod\n(使用hel-micro-core注入平台参数)"]
            createApis["创建API实例"]
            returnInstance["返回平台实例"]
            
            callCreateInstance --> injectPlatToMod
            injectPlatToMod --> createApis
            createApis --> returnInstance
        end
    end
    
    %% 外部依赖
    helMicroCore["hel-micro-core\n(核心功能)"]
    
    %% 依赖关系
    getCustomData -.-> helMicroCore
    injectPlatToMod -.-> helMicroCore
    
    style callGetNode fill:#ffe4c4,stroke:#333
    style callCreateInstance fill:#ffe4c4,stroke:#333
    style getCustomData fill:#e0ffff,stroke:#333
    style injectPlatToMod fill:#e0ffff,stroke:#333
    style returnShadowNode fill:#e0ffff,stroke:#333
    style returnInstance fill:#e0ffff,stroke:#333
