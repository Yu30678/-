# -
   
```mermaid
graph TB
    %% 定義節點
    User["👤 USER"]
    Firewall["🧱 Firewall"]
    Intranet["🌐 Intranet"]
    
    %% 前端層
    subgraph Frontend_Layer[" "]
        Frontend[".118.81:443<br/>ap.ar.acc frontend"]
    end
    
    %% 應用層 - 合併成一個框
    subgraph Application_Layer[" "]
        Backend[".118.82:443<br/>ap.ar backend"]
        HnraBackend[".118.83:443<br/>hnra / acc backend"]
        IMS[".118.84:443<br/>IMS"]
        SmartQuery[".118.87:443<br/>smart query"]
    end
    
    %% 路由與服務層
    subgraph Router_Service_Layer[" "]
        Router[".118.80:443<br/>router"]
        Auth[".118.86:443<br/>auth"]
        Shared[".118.85:443<br/>共用服務<br/>• job control<br/>• run job<br/>• mail<br/>• file<br/>• file export"]
    end
    
    %% 資料庫層
    subgraph Database_Layer[" "]
        MainDB[("🗄️ 資料庫<br/>.100.45:1433")]
        Informix[("🗄️ Informix<br/>.110.8:1531")]
        SunSystems[("🗄️ SunSystems<br/>:1433")]
    end
    
    %% 備援與儲存層
    subgraph Backup_Storage_Layer[" "]
        ActiveStandby1[("📀 .100.41<br/>active standby<br/>10G(目前2.9G)")]
        ActiveStandby2[("📀 .100.42<br/>active standby<br/>10G(目前2.9G)")]
        Storage[("💾 Storage")]
    end
    
    %% 用戶到網路層 - 加粗主要路徑
    User ==> Firewall
    Firewall ==> Intranet
    
    %% Intranet 到各服務 - 使用粗線
    Intranet ==> Frontend
    Intranet ==> HnraBackend
    Intranet ==> IMS
    Intranet ==> SmartQuery
    
    %% 前端與後端 - 雙向粗線
    Frontend <==> Backend
    
    %% 應用層到 Router - 橘紅色系
    Backend <==> Router
    HnraBackend <==> Router
    
    %% 服務層到 Router - 紫色系
    Shared <==> Router
    Auth <==> Router
    
    %% 應用層到資料庫 - 藍色系
    Backend <==> MainDB
    Backend <==> Informix
    HnraBackend <==> MainDB
    IMS <==> MainDB
    SmartQuery <==> MainDB
    SmartQuery <==> Informix
    
    %% 服務層到資料庫
    Auth <==> MainDB
    Shared <==> MainDB
    
    %% 資料庫複製 - 單向
    MainDB ==> ActiveStandby1
    MainDB ==> ActiveStandby2
    
    %% 備援到儲存 - 虛線
    ActiveStandby1 -.-> Storage
    ActiveStandby2 -.-> Storage
    
    %% 樣式定義
    classDef userStyle fill:#34495e,stroke:#2c3e50,color:#fff,stroke-width:4px,font-size:16px,font-weight:bold
    classDef networkStyle fill:#f39c12,stroke:#e67e22,color:#fff,stroke-width:3px,font-size:14px,rx:15,ry:15
    classDef vmStyle fill:#2c3e50,stroke:#34495e,color:#fff,stroke-width:2px,font-size:14px,rx:10,ry:10
    classDef dbStyle fill:#3498db,stroke:#2980b9,color:#fff,stroke-width:2px,font-size:14px
    classDef routerStyle fill:#e74c3c,stroke:#c0392b,color:#fff,stroke-width:3px,font-size:14px,rx:10,ry:10
    classDef serviceStyle fill:#9b59b6,stroke:#8e44ad,color:#fff,stroke-width:2px,font-size:14px,rx:10,ry:10
    classDef storageStyle fill:#27ae60,stroke:#229954,color:#fff,stroke-width:2px,font-size:14px
    
    class User userStyle
    class Intranet,Firewall networkStyle
    class Frontend,Backend,HnraBackend,IMS,SmartQuery vmStyle
    class Router routerStyle
    class Auth,Shared serviceStyle
    class MainDB,Informix,SunSystems,ActiveStandby1,ActiveStandby2,Storage dbStyle
    
    %% 子圖樣式 - 增加圓角和陰影效果
    style Frontend_Layer fill:#ecf0f1,stroke:#95a5a6,stroke-width:2px,stroke-dasharray:5 5,rx:20,ry:20
    style Application_Layer fill:#ecf0f1,stroke:#95a5a6,stroke-width:2px,stroke-dasharray:5 5,rx:20,ry:20
    style Router_Service_Layer fill:#fadbd8,stroke:#e74c3c,stroke-width:3px,rx:20,ry:20
    style Database_Layer fill:#d6eaf8,stroke:#3498db,stroke-width:3px,rx:20,ry:20
    style Backup_Storage_Layer fill:#d5f4e6,stroke:#27ae60,stroke-width:3px,rx:20,ry:20
    
    %% 連線樣式
    linkStyle 0,1 stroke:#34495e,stroke-width:4px
    linkStyle 2,3,4,5 stroke:#2c3e50,stroke-width:3px
    linkStyle 6 stroke:#2c3e50,stroke-width:3px
    linkStyle 7,8 stroke:#e74c3c,stroke-width:2px
    linkStyle 9,10 stroke:#9b59b6,stroke-width:2px
    linkStyle 11,12,13,14,15,16,17,18 stroke:#3498db,stroke-width:2px
    linkStyle 19,20 stroke:#3498db,stroke-width:2px
    linkStyle 21,22 stroke:#27ae60,stroke-width:2px,stroke-dasharray:5 5
