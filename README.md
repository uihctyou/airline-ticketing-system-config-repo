
# Airline Ticketing System Configuration Repository

![Logo](https://socialify.git.ci/uihctyou/airline-ticketing-system-config-repo/image?font=Source%20Code%20Pro&name=1&pattern=Circuit%20Board&theme=Dark)

[![GitHub License](https://img.shields.io/github/license/uihctyou/airline-ticketing-system-config-repo.svg?logo=github)](LICENSE)
![Version](https://img.shields.io/badge/airline_ticketing_system_config_repo-v1.0.25.401-pink)

This repository centralizes configurations for all microservices in the **Airline Ticketing System** via Spring Cloud Config Server.  

本仓库通过 Spring Cloud Config Server 集中管理 **航空票务系统** 所有微服务的配置文件。

<br/>

## Repository Structure / 仓库结构



### **Directory Conventions / 目录规范**
```
airline-ticketing-system-config-repo/
├── infrastructure/               # Infrastructure Layer 基础设施模块
│   └── airline-config-server/    # Config Server 配置中心
│       └── application.yml       # Remove (可移除 - 2025-04-02 updated）
│
└── services/                     # Business Services Layer 业务服务模块
    ├── airline-flight-service/   # Flight Service 航班服务
    │   ├── application-dev.yml   # Development Config 开发环境
    │   └── application-prod.yml  # Production Config 生产环境
    ├── airline-booking-service/  # Booking Service 预订服务
    │   └── application.yml
    └── airline-payment-service/  # Payment Service 支付服务
        └── application.yml
```

### **File Naming Rules / 文件命名规则**
- `{service-name}/application.yml`: 
Default configurations (shared across environments)  
  服务默认配置（所有环境共享）

- `{service-name}/application-{profile}.yml`: 
Environment-specific config (e.g., `dev`, `prod`)  
  特定环境配置（如 `dev`, `prod`）

- `bootstrap.yml`: 
Config Server self-configuration (e.g., Git URI)  
  Config Server 自身配置（如 Git 仓库地址）


<br/>

## Usage Guide / 使用说明

### **1. Configuration Fetching Rules / 配置拉取规则**
- **Service Name Matching**: Config Server maps `spring.application.name` to directory names.  
  **服务名匹配**: Config Server 根据 `spring.application.name` 匹配目录名。
  - Example: Service `airline-flight-service` loads configs from `services/airline-flight-service/`.  
    示例：服务 `airline-flight-service` 会加载 `services/airline-flight-service/` 下的配置。
- **Environment Profiles**: Specify environments via `spring.profiles.active`.  
  **环境标识**: 通过 `spring.profiles.active` 指定环境（如 `dev`, `prod`）。
  - Example: `spring.profiles.active=dev` loads `application-dev.yml`.  
    示例：`spring.profiles.active=dev` 会加载 `application-dev.yml`。

### **2. Dynamic Configuration Refresh / 动态刷新配置**
1. Modify config files and push changes to Git.  
   修改配置文件并提交到 Git 仓库。
   
2. Send a refresh request to the target service:  
   向目标服务发送刷新请求：
   ```bash
   POST http://{service-host}:{port}/actuator/refresh
   ```
3. Hot-reload configurations (requires `@RefreshScope` on Beans).  
   配置热生效（需在 Bean 上添加 `@RefreshScope`）。


<br/>

##  Configuration Examples / 配置示例

### **Eureka Server Config / Eureka 服务配置**
```yaml
# infrastructure/airline-eureka-server/application.yml
server:
  port: 8761

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```

### **Flight Service Dev Config / 航班服务开发环境配置**
```yaml
# services/airline-flight-service/application-dev.yml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/flight_db
    username: dev_user
    password: dev_password

logging:
  level:
    root: DEBUG
```


<br/>

## Important Notes / 注意事项

1. **Branch Management**: Default branch is `main`. Use `spring.cloud.config.server.git.default-label` for custom branches.  
   **分支管理**: 默认使用 `main` 分支，可通过 `spring.cloud.config.server.git.default-label` 指定其他分支。

2. **Sensitive Data Encryption**: Encrypt passwords with symmetric keys or integrate Vault.  
   **敏感信息加密**: 对密码等敏感数据使用对称加密或集成 Vault。

3. **Local Testing**: Verify repository accessibility before deployment:  
   **本地测试**: 启动前确保仓库可访问：
   ```bash
   git clone https://github.com/your-username/airline-ticketing-system-config-repo.git
   ```


<br/>

## License / 许可证
This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.  
本项目使用 MIT 许可证，详见 [LICENSE](LICENSE)。
