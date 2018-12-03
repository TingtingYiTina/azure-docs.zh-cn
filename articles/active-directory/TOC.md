# [Azure Active Directory 文档](index.md)

# 概述
## [什么是 Azure Active Directory？](fundamentals/active-directory-whatis.md)
## [关于 Azure 标识管理](fundamentals/identity-fundamentals.md)
## [了解 Azure 标识解决方案](fundamentals/understand-azure-identity-solutions.md)
## [关联 Azure 订阅](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [驻留和数据注意事项](fundamentals/active-directory-data-storage-eu.md)
## [常见问题](fundamentals/active-directory-faq.md)
## [新增功能](fundamentals/whats-new.md)


# 入门
## [注册 Azure AD Premium](fundamentals/active-directory-get-started-premium.md)
## [添加自定义域名](fundamentals/add-custom-domain.md)
## [配置公司品牌](fundamentals/customize-branding.md)
## [将用户添加到 Azure AD](fundamentals/add-users-azure-active-directory.md)
## [将许可证分配给用户](fundamentals/license-users-groups.md)
## [配置自助服务密码重置](authentication/quickstart-sspr.md)
## [在 Azure AD 中添加组织的隐私信息](active-directory-properties-area.md)
## [访问 Azure Active Directory 以创建新租户](fundamentals/active-directory-access-create-new-tenant.md)


# 如何
## 规划和设计
### [了解 Azure AD 体系结构](fundamentals/active-directory-architecture.md)
### [Azure Active Directory 中的声明映射](develop/active-directory-claims-mapping.md)
### [部署混合标识解决方案](hybrid/plan-hybrid-identity-design-considerations-overview.md)
#### 确定要求
##### [标识](hybrid/plan-hybrid-identity-design-considerations-business-needs.md)
##### [目录同步](hybrid/plan-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [多重身份验证](hybrid/plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [标识生命周期策略](hybrid/plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [数据安全性规划](hybrid/plan-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [数据保护](hybrid/plan-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [内容管理](hybrid/plan-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [访问控制](hybrid/plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [事件响应](hybrid/plan-hybrid-identity-design-considerations-incident-response-requirements.md)
#### 规划标识生命周期
##### [任务](hybrid/plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [采用策略](hybrid/plan-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [后续步骤](hybrid/plan-hybrid-identity-design-considerations-nextsteps.md)
#### [工具比较](hybrid/plan-hybrid-identity-design-considerations-tools-comparison.md)

## 管理用户
### [将新用户添加到 Azure AD](fundamentals/add-users-azure-active-directory.md)
### [管理用户个人资料](fundamentals/active-directory-users-profile-azure-portal.md)
### [重置用户密码](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [将用户分配到管理员角色](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [添加另一个目录中的来宾用户 (B2B)](b2b/what-is-b2b.md)
#### [管理员添加 B2B 用户](b2b/add-users-administrator.md)
#### [信息工作者添加 B2B 用户](b2b/add-users-information-worker.md)
#### [API 和自定义](b2b/customize-invitation-api.md)
#### [Google 联合身份验证](b2b/google-federation.md)
#### [代码和 Azure PowerShell 示例](b2b/code-samples.md)
#### [自助注册门户示例](b2b/self-service-portal.md)
#### [邀请电子邮件](b2b/invitation-email-elements.md)
#### [邀请兑换](b2b/redemption-experience.md)
#### [在没有邀请的情况下添加 B2B 用户](b2b/add-user-without-invite.md)
#### [允许或阻止邀请](b2b/allow-deny-list.md)
#### [用于 B2B 的条件性访问](b2b/conditional-access.md)
#### [B2B 共享策略](b2b/delegate-invitations.md)
#### [将 B2B 用户添加到角色](b2b/add-guest-to-role.md)
#### [动态组和 B2B 用户](b2b/use-dynamic-groups.md)
#### [离开组织](b2b/leave-the-organization.md)
#### [审核和报表](b2b/auditing-and-reporting.md)
#### [适用于混合组织的 B2B](b2b/hybrid-organizations.md)
##### [授予 B2B 用户对本地应用的访问权限](b2b/hybrid-cloud-to-on-premises.md)
##### [授予本地用户对云应用的访问权限](b2b/hybrid-on-premises-to-cloud.md)
#### [B2B 和 Office 365 外部共享](b2b/o365-external-user.md)
#### [B2B 许可](b2b/licensing-guidance.md)
#### [当前限制](b2b/current-limitations.md)
#### [常见问题](b2b/faq.md)
#### [对 B2B 进行故障排除](b2b/troubleshoot.md)
#### [了解 B2B 用户](b2b/user-properties.md)
#### [B2B 用户令牌](b2b/user-token.md)
#### [用于 Azure AD 集成应用的 B2B](b2b/configure-saas-apps.md)
#### [B2B 用户声明映射](b2b/claims-mapping.md)
#### [B2B 协作与 B2C 的比较](b2b/compare-with-b2c.md)
#### [获取 B2B 支持](b2b/get-support.md)

## [管理组和成员](fundamentals/active-directory-manage-groups.md)
### [管理组](fundamentals/active-directory-groups-create-azure-portal.md)
### [删除组及其成员](fundamentals/active-directory-groups-delete-group.md)
### [管理组设置](fundamentals/active-directory-groups-settings-azure-portal.md)
## [管理报表](reports-monitoring/overview-reports.md)
### [登录活动](reports-monitoring/concept-sign-ins.md)
### [审核活动](reports-monitoring/concept-audit-logs.md)
### [有风险的用户](reports-monitoring/concept-user-at-risk.md)
### [有风险的登录](reports-monitoring/concept-risky-sign-ins.md)
### [风险事件](reports-monitoring/concept-risk-events.md)
### [使用 Azure Monitor 监视日志](reports-monitoring/concept-activity-logs-in-azure-monitor.md)
### [常见问题](reports-monitoring/reports-faq.md)

### 任务
#### [下载登录报表](reports-monitoring/quickstart-download-sign-in-report.md)
#### [下载审核报表](reports-monitoring/quickstart-download-audit-report.md)
#### [配置命名位置](reports-monitoring/quickstart-configure-named-locations.md)
#### [查找活动报表](reports-monitoring/howto-find-activity-reports.md)
#### [使用 Azure AD Power BI 内容包](reports-monitoring/howto-power-bi-content-pack.md)
#### [修复已标记为存在风险的用户](reports-monitoring/howto-remediate-users-flagged-for-risk.md)
#### [将活动日志路由到 Azure 事件中心](reports-monitoring/quickstart-azure-monitor-stream-logs-to-event-hub.md)
#### [将活动日志存档到 Azure 存储帐户](reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
#### [使用 Azure Monitor 将活动日志与 Splunk 集成](reports-monitoring/tutorial-integrate-activity-logs-with-splunk.md)
#### [使用 Azure Monitor 将活动日志与 SumoLogic 集成](reports-monitoring/howto-integrate-activity-logs-with-sumologic.md)
#### [使用 Azure Monitor 将活动日志与 Log Analytics 集成](reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

### 引用
#### [保留](reports-monitoring/reference-reports-data-retention.md)
#### [延迟](reports-monitoring/reference-reports-latencies.md)
#### [审核活动参考](reports-monitoring/reference-audit-activities.md)
#### [登录活动错误代码](reports-monitoring/reference-sign-ins-error-codes.md)
#### [解释 Azure Monitor 中的审核日志架构](reports-monitoring/reference-azure-monitor-audit-log-schema.md)
#### [解释 Azure Monitor 中的登录日志架构](reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md)

### 故障排除
#### [在 Azure AD 活动日志中缺少数据](reports-monitoring/troubleshoot-missing-audit-data.md)
#### [在下载项中缺少数据](reports-monitoring/troubleshoot-missing-data-download.md)
#### [Azure AD 活动日志内容包错误](reports-monitoring/troubleshoot-content-pack.md)
#### [Azure AD 报告 API 中的错误](reports-monitoring/troubleshoot-graph-api.md)

### [以编程方式访问](reports-monitoring/concept-reporting-api.md)
#### [先决条件](reports-monitoring/howto-configure-prerequisites-for-reporting-api.md)
#### [使用证书](reports-monitoring/tutorial-access-api-with-certificates.md)

## [管理密码](authentication/concept-sspr-howitworks.md)

## 管理应用
### [概述](manage-apps/what-is-application-management.md)
### [入门](manage-apps/plan-an-application-integration.md)
### [SaaS 应用集成教程](saas-apps/tutorial-list.md)

### [对 SaaS 应用进行用户预配和取消预配](manage-apps/user-provisioning.md) 
#### [应用集成教程](saas-apps/tutorial-list.md) 
#### [对启用 SCIM 的应用自动执行预配](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [自定义属性映射](manage-apps/customize-application-attributes.md) 
#### [为属性映射编写表达式](manage-apps/functions-for-customizing-application-data.md) 
#### [使用范围筛选器](manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) 
#### [针对自动用户预配进行报告](manage-apps/check-status-user-account-provisioning.md) 
#### [排查用户预配问题](active-directory-application-provisioning-content-map.md) 

### [使用应用代理远程访问应用](manage-apps/application-proxy.md)
#### 入门
##### [启用应用代理](manage-apps/application-proxy-enable.md)
##### [发布应用](manage-apps/application-proxy-publish-azure-portal.md)
##### [自定义域](manage-apps/application-proxy-configure-custom-domain.md)
#### [单一登录](manage-apps/application-proxy-single-sign-on.md)
##### [使用 KCD 执行 SSO](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
##### [使用标头执行 SSO](manage-apps/application-proxy-configure-single-sign-on-with-ping-access.md)
##### [将 SSO 与密码保管配合使用](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md)
#### 概念
##### [连接器](manage-apps/application-proxy-connectors.md)
##### [安全性](manage-apps/application-proxy-security.md)
##### [网络](manage-apps/application-proxy-network-topology.md)


##### [从 TMG 或 UAG 升级](manage-apps/application-proxy-migration.md)

#### 高级配置
##### [在单独的网络上发布](manage-apps/application-proxy-connector-groups.md)
##### [代理服务器](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
##### [声明感知应用](manage-apps/application-proxy-configure-for-claims-aware-applications.md)
##### [本机客户端应用](manage-apps/application-proxy-configure-native-client-application.md)
##### [无提示安装](manage-apps/application-proxy-register-connector-powershell.md)
##### [自定义主页](manage-apps/application-proxy-configure-custom-home-page.md)
##### [转换内联链接](manage-apps/application-proxy-configure-hard-coded-link-translation.md)
##### [通配符](manage-apps/application-proxy-wildcard.md)
##### [删除个人数据](manage-apps/application-proxy-remove-personal-data.md)


#### 发布演练
##### [远程桌面](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](manage-apps/application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [故障排除](manage-apps/application-proxy-troubleshoot.md)

### 管理企业应用
#### [添加应用程序](manage-apps/add-application-portal.md)
#### [查看租户应用](manage-apps/view-applications-portal.md)
#### [配置单一登录](manage-apps/configure-single-sign-on-portal.md)
#### [分配用户](manage-apps/assign-user-or-group-access-portal.md)
#### [自定义品牌](manage-apps/change-name-or-logo-portal.md)
#### [禁用用户登录](manage-apps/disable-user-sign-in-portal.md)
#### [删除用户](manage-apps/remove-user-or-group-access-portal.md)

#### [管理用户帐户预配](manage-apps/configure-automatic-user-provisioning-portal.md)

#### [SAML 应用的高级证书签名](manage-apps/certificate-signing-options.md)
#### [从用户体验中隐藏应用程序](manage-apps/hide-application-from-user-portal.md)
### [使用 HRD 策略配置登录自动加速](manage-apps/configure-authentication-for-federated-users-portal.md)
### [将 AD FS 应用迁移到 Azure AD](manage-apps/migrate-adfs-apps-to-azure.md) 
### [管理对应用的访问权限](manage-apps/what-is-access-management.md)
#### [SSO 访问](manage-apps/what-is-single-sign-on.md)
#### [SSO 证书](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [租户限制](manage-apps/tenant-restrictions.md)
#### [使用 SCIM 预配用户](manage-apps/use-scim-to-provision-users-and-groups.md)

### [了解 Azure AD 应用程序许可体验](develop/application-consent-experience.md)

### 故障排除



#### 访问面板
##### [应用未出现](manage-apps/access-panel-troubleshoot-application-not-appearing.md)
##### [出现意外的应用](manage-apps/access-panel-troubleshoot-unexpected-application.md)
##### [无法登录](manage-apps/access-panel-troubleshoot-web-sign-in-problem.md)
##### [安装浏览器扩展时出错](manage-apps/access-panel-extension-problem-installing.md)
##### [如何使用自助服务应用访问权限](manage-apps/access-panel-manage-self-service-access.md)
##### [使用自助服务应用访问权限时出错](manage-apps/access-panel-troubleshoot-self-service-access.md)

#### 添加应用
##### [选择应用类型](manage-apps/choose-application-type.md)
##### [常见问题 - 库应用](manage-apps/adding-gallery-app-common-problems.md)
##### [常见问题 - 非库应用](manage-apps/adding-non-gallery-app-common-problems.md)

#### 应用程序代理
##### [显示应用页时出现问题](manage-apps/application-proxy-page-appearance-broken-problem.md)
##### [应用程序加载时间过长](manage-apps/application-proxy-page-load-speed-problem.md)
##### [应用程序页上的链接不起作用](manage-apps/application-proxy-page-links-broken-problem.md)
##### [要为应用打开哪些端口](manage-apps/application-proxy-connectivity-ports-how-to.md)
##### [应用的连接器组中没有起作用的连接器](manage-apps/application-proxy-connectivity-no-working-connector.md)
##### [在管理门户中配置](manage-apps/application-proxy-config-how-to.md)
##### [为应用配置单一登录](manage-apps/application-proxy-config-sso-how-to.md)
##### [在管理门户中创建应用时出现问题](manage-apps/application-proxy-config-problem.md)
##### [配置 Kerberos 约束委派](manage-apps/application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [使用 PingAccess 配置](manage-apps/application-proxy-back-end-ping-access-how-to.md)
##### [“无法访问此企业应用程序”错误](manage-apps/application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [安装应用程序代理程序连接器时出现问题](manage-apps/application-proxy-connector-installation-problem.md)


#### 应用程序注册
##### [为应用程序对象填写字段](develop/registration-config-specific-application-property-how-to.md)
##### [更改令牌生存期默认值](develop/registration-config-change-token-lifetime-how-to.md)

#### 身份验证
##### [配置终结点](develop/registration-config-how-to.md)

#### 条件性访问
##### [客户不符合设备注册先决条件](active-directory-conditional-access.md)
##### [企业网络外部规则如何以及何时生效？](https://aka.ms/calocation)
##### [如何增加允许用户在 Azure AD 中注册的设备数？](active-directory-azureadjoin-setup.md)
##### [如何为 Exchange Online 设置条件访问？](https://aka.ms/csforexchange)
##### [如何为 Windows 7 设备设置条件访问？](active-directory-conditional-access.md#device-based-conditional-access)
##### [哪些应用程序支持条件访问？](active-directory-conditional-access-supported-apps.md)

#### 查找 API
##### [查找 API](develop/api-find-an-api-how-to.md)

#### 管理访问权限
##### [为应用分配用户和组](manage-apps/methods-for-assigning-users-and-groups.md)
##### [删除对应用的用户访问权限](manage-apps/methods-for-removing-user-access.md)
##### [配置自助服务应用分配](manage-apps/manage-self-service-access.md)
##### [分配了意外的用户](manage-apps/ways-users-get-assigned-to-applications.md)
##### [应用程序列表中的意外应用](manage-apps/application-types.md)

#### 多租户应用
##### [配置新应用](develop/setup-multi-tenant-app.md)
##### [添加到应用库](develop/registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### 权限
##### [选择 API 的权限](develop/perms-for-given-api.md)
##### [委派权限与应用程序权限](develop/delegated-and-app-perms.md)
##### [应用程序许可](develop/consent-framework.md)

#### 设置
##### [花费多长时间](manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [花费数小时 - 库应用](manage-apps/application-provisioning-when-will-provisioning-finish.md)
##### [配置用户预配 - 库应用](manage-apps/application-provisioning-config-how-to.md)
##### [配置用户预配时出现问题 - 库应用](manage-apps/application-provisioning-config-problem.md)
##### [配置用户预配时保存管理员凭据出现问题 - 库应用](manage-apps/application-provisioning-config-problem-storage-limit.md)
##### [未预配用户 - 库应用](manage-apps/application-provisioning-config-problem-no-users-provisioned.md)
##### [预配了错误的用户 - 库应用](manage-apps/application-provisioning-config-problem-wrong-users-provisioned.md)

#### 单一登录
##### [选择方法](manage-apps/single-sign-on-modes.md)
##### [配置](develop/registration-config-sso-how-to.md)
##### [配置联合单一登录 - 库应用](manage-apps/configure-federated-single-sign-on-gallery-applications.md)
##### [配置联合单一登录时常见的问题 - 库应用](manage-apps/configure-federated-single-sign-on-gallery-applications-problems.md)
##### [配置联合单一登录 - 非库应用](manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
##### [配置联合单一登录时常见的问题 - 非库应用](manage-apps/configure-federated-single-sign-on-non-gallery-applications-problems.md)
##### [配置密码 - 库应用](manage-apps/configure-password-single-sign-on-gallery-applications.md)
##### [配置密码时常见的问题 - 库应用](manage-apps/configure-password-single-sign-on-gallery-applications-problems.md)
##### [配置密码 - 非库应用](manage-apps/configure-password-single-sign-on-non-gallery-applications.md)
##### [配置密码时常见的问题 - 非库应用](manage-apps/configure-password-single-sign-on-non-gallery-applications-problems.md)

#### 用户登录问题
##### [意外的许可提示](manage-apps/application-sign-in-unexpected-user-consent-prompt.md)
##### [用户许可错误](manage-apps/application-sign-in-unexpected-user-consent-error.md)
##### [从自定义门户登录时出现问题](manage-apps/application-sign-in-other-problem-deeplink.md)
##### [从访问面板登录时出现问题](manage-apps/application-sign-in-other-problem-access-panel.md)
##### [应用程序登录页上的错误](manage-apps/application-sign-in-problem-application-error.md)
##### [使用密码单一登录时出现的问题 - 非库应用](manage-apps/application-sign-in-problem-password-sso-non-gallery.md)
##### [使用密码单一登录时出现的问题 - 库应用](manage-apps/application-sign-in-problem-password-sso-gallery.md)
##### [登录到 Microsoft 应用时出现问题](manage-apps/application-sign-in-problem-first-party-microsoft.md)
##### [使用联合单一登录时出现的问题 - 非库应用](manage-apps/application-sign-in-problem-federated-sso-non-gallery.md)
##### [使用联合单一登录时出现的问题 - 库应用](manage-apps/application-sign-in-problem-federated-sso-gallery.md)
##### [登录自定义开发的应用时出现的问题](manage-apps/application-sign-in-problem-custom-dev.md)
##### [登录本地应用时出现的问题 - 应用程序代理](manage-apps/application-sign-in-problem-on-premises-application-proxy.md)


## 管理目录
### [Azure AD Connect](hybrid/whatis-hybrid-identity.md)
### 自定义域名
#### [快速入门](fundamentals/add-custom-domain.md)
### [管理目录](fundamentals/active-directory-administer.md)
### [使用 Azure AD Connect 集成本地标识](hybrid/whatis-hybrid-identity.md)

### [配置令牌生存期](develop/active-directory-configurable-token-lifetimes.md)

## 保护标识

### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)



## [故障排除](fundamentals/active-directory-troubleshooting-support-howto.md)

## 部署 Azure AD 概念证明 (PoC)
### [PoC 演练手册：简介](active-directory-playbook-intro.md)
### [PoC 演练手册：要素](active-directory-playbook-ingredients.md)
### [PoC 演练手册：实现](active-directory-playbook-implementation.md)
### [PoC 演练手册：构建基块](active-directory-playbook-building-blocks.md)

# 引用
## [代码示例](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [Azure PowerShell cmdlet](/powershell/azure/overview)
## [Java API 参考](/java/api)
## [.NET API](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)

# 相关内容
## [多重身份验证](/azure/multi-factor-authentication/)
## [Azure AD Connect](hybrid/whatis-hybrid-identity.md)
## [Azure AD Connect Health](hybrid/whatis-hybrid-identity-health.md)
## [适用于开发人员的 Azure AD](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/pim-configure.md)

# 资源
## [Azure AD 部署计划](./fundamentals/active-directory-deployment-plans.md)
## [Azure 反馈论坛](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure 路线图](https://azure.microsoft.com/roadmap/?category=security-identity)
## [MSDN 论坛](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [定价](https://azure.microsoft.com/pricing/details/active-directory/)
## [定价计算器](https://azure.microsoft.com/pricing/calculator/)
## [服务更新](https://azure.microsoft.com/updates/?product=active-directory)
## [堆栈溢出](https://stackoverflow.com/questions/tagged/azure-active-directory)
## [视频](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
