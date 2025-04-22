# onedrive 策略

# IT 管理员 - 使用 OneDrive 策略控制同步设置

## 本文内容

1. [使用组策略管理 OneDrive](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#manage-onedrive-using-group-policy)
2. [按字符串 ID 显示的策略列表](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#list-of-policies-by-string-id)
3. [计算机配置策略](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#computer-configuration-policies)
4. [用户配置策略](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#user-configuration-policies)
5. [另请参阅](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#see-also)

本文介绍管理员可使用 组策略 或使用 Microsoft Intune [中的管理模板](https://learn.microsoft.com/zh-cn/sharepoint/configure-sync-intune)配置 (GPO) OneDrive 组策略 对象。 可以使用本文中的注册表项信息来确认已启用设置。

 备注

如果你不是 IT 管理员，请参阅[在 Windows 中使用新的 OneDrive 同步 应用同步文件](https://support.office.com/article/615391c4-2bd3-4aae-a42a-858262e42a49)，了解有关OneDrive 同步设置的信息。

<iframe src="https://www.microsoft.com/zh-cn/videoplayer/embed/RE2CnSx?postJsllMsg=true&amp;autoCaptions=zh-cn" data-src="https://www.microsoft.com/zh-cn/videoplayer/embed/RE2CnSx?postJsllMsg=true&amp;autoCaptions=zh-cn" frameborder="0" allowfullscreen="true" data-linktype="external" title="视频播放器"></iframe>

## 使用组策略管理 OneDrive

1. 安装适用于 Windows 的 OneDrive 同步应用。 (若要查看发布的版本和下载版本，请参阅 [发行说明](https://support.office.com/article/845dcf18-f921-435e-bf28-4e24b95e5fc0?)。) 安装同步应用下载 .adml 和 .admx 文件。
2. 浏览到 `%localappdata%\Microsoft\OneDrive\\*BuildNumber*\adm\`​ (，以便 [每台计算机同步应用](https://learn.microsoft.com/zh-cn/sharepoint/per-machine-installation) 浏览到 `%ProgramFiles(x86)%\Microsoft OneDrive\BuildNumber\adm\`​ 或 `%ProgramFiles%\Microsoft OneDrive\BuildNumber\adm\`​ (，具体取决于操作系统体系结构) ) 到语言的子文件夹，根据需要 (*，其中 BuildNumber* 是 **“关于** ”选项卡) 上的同步应用设置中显示的数字。  
    ​![OneDrive 安装目录中的 ADM 文件夹|347](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/85e0fe3f-84eb-4a29-877f-c706dda4d075.png)​
3. 复制 .adml 和 .admx 文件。
4. 将 .admx 文件粘贴到域的中央存储中， `\\\\*domain*\sysvol\domain\Policies\PolicyDefinitions`​ (*域* 是你的域名（如 corp.contoso.com) ），并将 .adml 文件粘贴到相应的语言子文件夹中，例如 en-us。 如果 PolicyDefinitions 文件夹不存在，请参阅[如何在 Windows 中创建和管理组策略管理模板的中央存储](https://support.microsoft.com/help/3087759)，或在 下`%windir%\policydefinitions`​使用本地策略存储。
5. 运行[远程服务器管理工具](https://learn.microsoft.com/zh-cn/windows-server/remote/remote-server-administration-tools)，在域控制器或 Windows 计算机上配置设置。
6. (站点、域或组织单位) ，将 GPO 链接到 Active Directory 容器。 有关详细信息，请参阅[将组策略对象链接到 Active Directory 容器](https://learn.microsoft.com/zh-cn/previous-versions/windows/desktop/Policy/linking-gpos-to-active-directory-containers)。
7. 使用安全筛选来缩小设置范围。 默认情况下，设置应用于与其关联的容器内的所有用户和计算机对象。不过，可使用安全筛选将策略的应用范围缩小到一部分用户或计算机。 有关详细信息，请参阅 [筛选 GPO 的范围](https://learn.microsoft.com/zh-cn/previous-versions/windows/desktop/Policy/filtering-the-scope-of-a-gpo)。

OneDrive GPO 的工作原理是在域中的计算机上设置注册表项。

* 当你启用或禁用设置时，域中计算机上的相应注册表项也会随之更新。 如果稍后将设置改回 **“未配置**”，则不会修改相应的注册表项，并且更改不会生效。 配置完设置后，请将其设为“**启用**”或“**禁用**”以继续。
* 写入注册表项的位置已更新。 使用最新文件时，可能会删除先前设置的注册表项。

 备注

有关存储的信息，请参阅[适用于 Windows 10 的 OneDrive 文件随选和存储感知](https://support.office.com/article/de5faa9a-6108-4be1-87a6-d90688d08a48)和[策略 CSP - 存储](https://learn.microsoft.com/zh-cn/windows/client-management/mdm/policy-csp-storage)。

## 按字符串 ID 显示的策略列表

* （AllowTenantList） [仅允许同步特定组织的 OneDrive 帐户](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#allow-syncing-onedrive-accounts-for-only-specific-organizations)
* （AutomaticUploadBandwidthPercentage） [将同步应用上传速率限制为吞吐量的一定百分比](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#limit-the-sync-app-upload-rate-to-a-percentage-of-throughput)
* (BlockExternalListSync) 此设置控制列表同步，为方便起见在此处列出。 有关详细信息，请参阅[阻止用户同步从其他组织共享的列表](https://learn.microsoft.com/zh-cn/sharepoint/lists-sync-policies#prevent-users-from-syncing-lists-shared-from-other-organizations)。
* （BlockExternalSync） [防止用户同步从其他组织共享的库和文件夹](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prevent-users-from-syncing-libraries-and-folders-shared-from-other-organizations)
* （BlockTenantList） [阻止同步特定组织的 OneDrive 帐户](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#block-syncing-onedrive-accounts-for-specific-organizations)
* （DefaultRootDir） [设置 OneDrive 文件夹的默认位置](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#set-the-default-location-for-the-onedrive-folder)
* （DehydrateSyncedTeamSites） [将已同步的团队网站文件转换为仅联机文件](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#convert-synced-team-site-files-to-online-only-files)
* (DisableAutoConfig) [防止自动进行身份验证](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prevent-authentication-from-automatically-happening)
* （DisableCustomRoot） [阻止用户更改其 OneDrive 文件夹的位置](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prevent-users-from-changing-the-location-of-their-onedrive-folder)
* （DisableFirstDeleteDialog） [隐藏“从所有位置移除已删除文件”提醒](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#hide-the-deleted-files-are-removed-everywhere-reminder)
* (DisableNewAccountDetection) [隐藏消息以同步 Consumer OneDrive 文件](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#hide-the-messages-to-sync-consumer-onedrive-files)
* (DisableNucleusSilentConfig) 此设置控制列表同步，为方便起见在此处列出。 有关详细信息，请参阅[阻止用户使用其 Windows 凭据以无提示方式登录到 Lists 同步](https://learn.microsoft.com/zh-cn/sharepoint/lists-sync-policies#prevent-users-from-getting-silently-signed-in-to-lists-sync-with-their-windows-credentials)。
* (DisableNucleusSync) 此设置控制列表同步，为方便起见在此处列出。 有关详细信息，请参阅[阻止 Lists 同步在设备上运行](https://learn.microsoft.com/zh-cn/sharepoint/lists-sync-policies#prevent-lists-sync-from-running-on-the-device)。
* （DisablePauseOnBatterySaver） [在设备打开省电模式时继续同步](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#continue-syncing-when-devices-have-battery-saver-mode-turned-on)
* （DisablePauseOnMeteredNetwork） [在使用按流量计费的网络时继续同步](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#continue-syncing-on-metered-networks)
* （DisablePersonalSync） [阻止用户同步个人 OneDrive 帐户](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prevent-users-from-syncing-personal-onedrive-accounts)
* (DisableTutorial) [禁用在 OneDrive 安装程序结束时显示的教程](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#disable-the-tutorial-that-appears-at-the-end-of-onedrive-setup)
* （DiskSpaceCheckThresholdMB） [设置可自动下载的用户 OneDrive 的最大大小](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#set-the-maximum-size-of-a-users-onedrive-that-can-download-automatically)
* (DownloadBandwidthLimit) [将同步应用下载速度限制为固定速率](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#limit-the-sync-app-download-speed-to-a-fixed-rate)
* （EnableAllOcsiClients） [在 Office 桌面应用中合著和共享](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#coauthor-and-share-in-office-desktop-apps)
* （EnableAutomaticUploadBandwidthManagement） [为 OneDrive 启用自动上传带宽管理功能](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#enable-automatic-upload-bandwidth-management-for-onedrive)
* (EnableHoldTheFile) [允许用户选择如何处理 Office 文件同步冲突](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#allow-users-to-choose-how-to-handle-office-file-sync-conflicts)
* （EnableODIgnoreListFromGPO） [排除上传特定类型的文件](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#exclude-specific-kinds-of-files-from-being-uploaded)
* (EnableSyncAdminReports) [为 OneDrive 启用同步运行状况报告](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#enable-sync-health-reporting-for-onedrive)
* （FilesOnDemandEnabled） [使用 OneDrive 文件随选](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#use-onedrive-files-on-demand)
* （ForcedLocalMassDeleteDetection） [要求用户确认大型删除操作](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#require-users-to-confirm-large-delete-operations)
* （GPOSetUpdateRing） [设置同步应用更新通道](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#set-the-sync-app-update-ring)
* (KFMBlockOptIn) [阻止用户将 Windows 已知文件夹移动到 OneDrive](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prevent-users-from-moving-their-windows-known-folders-to-onedrive)
* （KPCBlockOptOut） [阻止用户将其 Windows 已知文件夹重定向到电脑](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prevent-users-from-redirecting-their-windows-known-folders-to-their-pc)
* （KFMOptInWithWizard） [提示用户将 Windows 已知文件夹移动到 OneDrive](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prompt-users-to-move-windows-known-folders-to-onedrive)
* （KTFSilentOptIn） [将 Windows 已知文件夹以无提示方式移动到 OneDrive](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#silently-move-windows-known-folders-to-onedrive)
* （LocalMassDeleteFileDeleteThreshold） [当用户在其本地计算机上删除多个 OneDrive 文件时提示用户](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prompt-users-when-they-delete-multiple-onedrive-files-on-their-local-computer)
* （MinSpaceSpaceInitInMB） [用户磁盘空间不足时阻止文件下载](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#block-file-downloads-when-users-are-low-on-disk-space)
* （AllowDisablePermissionInheritance） [允许 OneDrive 在文件夹同步只读中禁用 Windows 权限继承](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#allow-onedrive-to-disable-windows-permission-inheritance-in-folders-synced-read-only)
* （PreventNetworkTrafficPreUserSignIn） [在用户登录前阻止同步应用生成网络流量](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prevent-the-sync-app-from-generating-network-traffic-until-users-sign-in)
* （SharePointOnPremFrontDoorUrl）指定 SharePoint Server URL 和组织名称。 此设置适用于具有 SharePoint Server 2019 的客户。 有关将新 OneDrive 同步应用与 SharePoint Server 2019 一并使用的信息，请参阅 [使用新的 OneDrive 同步应用进行同步](https://learn.microsoft.com/zh-cn/SharePoint/install/configure-syncing-with-the-onedrive-sync-app/)。
* （SharePointOnPremPrioritization）在混合环境中指定 OneDrive 位置。 此设置适用于具有 SharePoint Server 2019 的客户。 有关将新 OneDrive 同步应用与 SharePoint Server 2019 一并使用的信息，请参阅 [使用新的 OneDrive 同步应用进行同步](https://learn.microsoft.com/zh-cn/SharePoint/install/configure-syncing-with-the-onedrive-sync-app/)。
* （SilentAccountConfig） [让客户使用 Windows 凭据以无提示方式登录 OneDrive 同步应用](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#silently-sign-in-users-to-the-onedrive-sync-app-with-their-windows-credentials)
* （TenantAutoMount） [配置要自动同步的团队网站库](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#configure-team-site-libraries-to-sync-automatically)
* （UploadBandwidthLimit） [将同步应用上传速度限制为固定速率](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#limit-the-sync-app-upload-speed-to-a-fixed-rate)
* （WarningMinSpaceSpaceInitInMB） [向磁盘空间不足的用户发出警告](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#warn-users-who-are-low-on-disk-space)

## 计算机配置策略

在“计算机配置\策略\管理模板\OneDrive”下，导航到 **“计算机配置 &gt; 策略**”。

​![组策略管理编辑器中的计算机配置策略](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/07b81d35-9ccc-4c61-8a86-52d9bcff7ddb.png)​

### 允许 OneDrive 在文件夹同步只读中禁用 Windows 权限继承

此设置允许 OneDrive 同步应用可以删除在用户电脑上同步的只读文件夹中的所有继承权限。 删除继承的权限可以提高同步用户具有只读权限的文件夹时同步应用的性能。

为用户启用此设置不会更改其在 SharePoint 中查看或编辑内容的权限。

建议不要为不同步只读内容的用户设置此策略。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"PermitDisablePermissionInheritance"=dword:00000001`​

### 仅允许同步特定组织的 OneDrive 帐户

此设置通过指定允许的租户 ID 列表来阻止用户将文件轻松上传至其他组织。

如果启用此设置，如果用户尝试从组织添加不允许的帐户，则会收到错误。 如果用户已添加此帐户，文件会停止同步。

要输入租户 ID，请在“**选项**”框中选择“**显示**”。

此策略设置以下注册表项：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive\AllowTenantList] "1111-2222-3333-4444"`​

（其中，“1111-2222-3333-4444”是[租户 ID](https://learn.microsoft.com/zh-cn/sharepoint/find-your-office-365-tenant-id)）。

此设置的优先级高于策略“[阻止同步特定组织的 OneDrive 帐户](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#block-syncing-onedrive-accounts-for-specific-organizations)”。 不要同时启用这两个设置。

### 用户磁盘空间不足时阻止文件下载

使用此设置，你可以指定最小的可用磁盘空间量，并阻止 OneDrive 同步应用 (OneDrive.exe) 在用户的磁盘空间少于此数量时下载文件。

系统会提示用户选择相应的选项以帮助释放空间。

启用此策略会将下面的注册表项值设置为介于 0 和 10240000 之间的数字：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive] "MinDiskSpaceLimitInMB"=dword:00000000`​

### 阻止同步特定组织的 OneDrive 帐户

通过此设置，可指定阻止的租户 ID 列表来阻止用户将文件上传至其他组织。

如果启用此设置，用户会在尝试添加被阻止的组织中的帐户时看到错误消息。 如果用户已添加此帐户，文件会停止同步。

要输入租户 ID，请在“**选项**”框中选择“**显示**”。

此策略设置以下注册表项。

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive\BlockTenantList] "1111-2222-3333-4444"`​

（其中，“1111-2222-3333-4444”是[租户 ID](https://learn.microsoft.com/zh-cn/sharepoint/find-your-office-365-tenant-id)）。

如果 [仅允许为特定组织同步 OneDrive 帐户](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#allow-syncing-onedrive-accounts-for-only-specific-organizations) ，则此设置不起作用。 不要同时启用这两个设置。

### 将同步的团队网站文件转换为仅联机文件

使用此设置，可以在启用 OneDrive 文件随选后，将已同步 SharePoint 文件转换为仅联机文件。 如果有多台电脑同步同一团队网站，那么启用此设置有助于最大限度地减少网络流量和本地存储空间使用。

如果启用此设置，当前同步的团队网站中的文件会默认更改为仅联机文件。 团队网站中稍后添加或更新的文件也会下载为仅联机文件。 要使用此设置，计算机必须运行 Windows 10 Fall Creators Update（版本 1709）或更高版本，且必须启用 OneDrive 文件随选。 未为本地 SharePoint 网站启用此功能。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"DehydrateSyncedTeamSites"=dword:00000001`​

有关查询和设置文件和文件夹状态的信息，请参阅 [查询和设置文件按需状态](https://learn.microsoft.com/zh-cn/sharepoint/files-on-demand-mac)。

### 为 OneDrive 启用自动上传带宽管理功能

此设置仅允许 OneDrive 同步应用 (OneDrive.exe) 在未使用的带宽可用时在后台上传数据。 它会阻止同步应用干扰正在使用网络的其他应用。 此设置由 Windows LEDBAT（低额外延迟后台传输）协议提供支持。 当 LEDBAT 检测到表示其他 TCP 连接正在占用带宽的延迟增加情况时，同步应用将减少自身的占用量以防止干扰。 在网络延迟再次降低且带宽被释放出来后，同步应用将提高上传速率并占用未使用的带宽。

如果启用此设置，同步应用上传速率将设置为根据带宽可用性 **自动调整** ，用户将无法更改它。

如果未配置此设置，用户可以选择将上传速率限制为固定值 (KB/秒) ，或将其设置为 **自动调整**。

 重要

如果启用或禁用此设置，然后将其改回 **“未配置”**，则最后一个配置将保持有效。 建议启用此设置，而不是 **将同步应用上传速度限制为固定速率**。 请勿同时启用这两个设置。 如果在同一设备上启用同步应用上传速率，此设置将覆盖 **将同步应用上传速率限制为吞吐量的百分比** 。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\Software\Policies\Microsoft\OneDrive]"EnableAutomaticUploadBandwidthManagement"=dword:00000001`​

### 为 OneDrive 启用同步运行状况报告

此设置允许 OneDrive 同步应用报表同步设备，以及管理同步报表中包含的运行状况数据。

如果启用此设置，OneDrive 同步应用将报告设备和运行状况数据，以包含在管理同步报告中。 必须在要从中获取报表的设备上启用此设置。

如果禁用或未配置此设置，OneDrive 同步应用设备和运行状况数据不会出现在管理员报告中。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"EnableSyncAdminReports"=dword:00000001`​

### 排除上传特定类型的文件

通过此设置可以输入关键字，以阻止 OneDrive 同步应用 (OneDrive.exe) 将某些文件上传到 OneDrive 或 SharePoint。 可以输入完整名称（如“setup.exe”），或使用星号 (*) 作为通配符来表示一系列字符，例如 *.pst。 关键字不区分大小写。

 备注

OneDrive 同步 应用不会同步 .tmp 和 .ini 文件。

如果启用此设置，同步应用不会上传匹配指定关键字的新文件。 跳过的文件不会显示错误，且文件仍保留在本地 OneDrive 文件夹中。

 备注

此设置将仅阻止匹配规范的文件。 它不适用于重命名为匹配指定关键字的现有文件。 此外，还会阻止在同步文件夹中创建且命名以匹配指定关键字的新文件。此外，也不会阻止在同步文件夹中创建，且命名匹配指定关键字的新文件。

在文件资源管理器中，文件在**“状态**”列中显示“已从同步中排除”图标。 启用此设置后，必须重启 OneDrive 同步应用，设置才会生效。

​![文件资源管理器中的“从同步中排除” 图标](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/excluded-from-sync.png)​

“OneDrive 活动中心”中还将显示一条消息，说明文件未同步的原因。

​![“管理员已排除同步这些文件类型”消息](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/excluded-files.png)​

 备注

用户仍然可以在 Web 浏览器中浏览到其 OneDrive，以上传已从其本地 OneDrive 文件夹中排除的文件。 我们建议用户在执行此上传后删除本地文件，因为在同一文件夹中具有相同名称的文件将导致与跳过的文件发生同步冲突。

如果禁用或未配置此设置，则将上传所有同步文件夹中所有支持的文件。

启用此策略会创建以下路径中的字符串列表：

​`HKLM\SOFTWARE\Policies\Microsoft\OneDrive\EnableODIgnoreListFromGPO`​

 备注

此设置为你提供的灵活性高于管理中心中的[阻止同步特定文件类型设置](https://learn.microsoft.com/zh-cn/sharepoint/block-file-types)。 此外，使用此设置时，用户不会看到已排除文件的错误。  
此设置不支持上传要排除的 Office 文件。 支持所有其他文件类型。

### 隐藏“从所有位置移除已删除文件”提醒

当用户从同步位置删除本地文件时，将显示一条警告消息，指出文件将不再在用户的所有设备和 Web 上可用。 此设置允许隐藏警告消息。

​![](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/deleted-files-removed-everywhere.png)​

如果启用此设置，则当用户在本地 **删除文件时** ，不会在任何地方看到已删除的文件提醒。 （当用户从已同步的团队网站中删除文件时，此提醒称为"删除的文件给所有人"。）

如果禁用或未配置此设置，则会显示提醒，直到用户选择“ **不再显示此提醒**”。

启用此策略将以下注册表项值设置为 1： `HKLM\SOFTWARE\Policies\Microsoft\OneDrive\DisableFirstDeleteDialog =dword:00000001`​

### 隐藏消息以同步使用者 OneDrive 文件

此设置确定是否提示用户使用检测到的已知 Microsoft 帐户 (MSA) 同步其使用者文件。

**启用**：如果要禁止向用户显示消息，但允许用户手动配置其使用者帐户以与其 OneDrive 使用者文件同步，请启用此设置。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"DisableNewAccountDetection"=dword:00000001`​

**禁用**：禁用或不要配置此设置，以允许用户收到同步其使用者文件的提示。

### 将同步应用上传速率限制为吞吐量的一定百分比

使用此设置，你可以指定 OneDrive 同步应用可用于上传文件的计算机上传吞吐量的百分比，以此平衡计算机上不同上传任务的性能。 将此吞吐量设置为百分比可让同步应用响应吞吐量的增加和减少。 设置的百分比越低，上传文件的速度越慢。 建议设置不小于 50% 的值。 同步应用会定期上传（没有一分钟限制），然后降低至设定的上传百分比。 此模式允许小文件快速上传，同时防止大型上传主导计算机的上传吞吐量。 建议在推出[将 Windows 已知文件夹以无提示方式移动到 OneDrive](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#silently-move-windows-known-folders-to-onedrive)或[“提示用户将 Windows 已知文件夹移动到 OneDrive”](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prompt-users-to-move-windows-known-folders-to-onedrive)时启用此设置，以控制上传已知文件夹内容产生的网络影响。

​![上传吞吐量计算](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/limit-upload-rate-percentage-throughput.png)​

 备注

同步应用检测到的最大吞吐量值有时可能会高于或低于预期，这是因为 Internet 服务提供商 (ISP) 使用的流量限制机制可能不同。  
有关估算同步所需的网络带宽的信息，请参阅 [OneDrive 同步 应用的网络利用率规划](https://learn.microsoft.com/zh-cn/sharepoint/network-utilization-planning)。

如果启用此设置并在“ **带宽** ”框中输入 10-99) (百分比，则计算机将使用将文件上传到 OneDrive 时指定的上传吞吐量百分比，用户无法更改它。

启用此策略会将以下注册表项设置为以下示例中所述的值：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"AutomaticUploadBandwidthPercentage"=dword:00000032`​

上述注册表项使用 50 对应的十六进制值（即 00000032），将上传吞吐量百分比设置为 50%。

如果禁用或未配置此设置，用户可以选择将上传速率限制为固定值 (kb/秒) ，或将其设置为 **“自动调整**”，从而将上传速率设置为吞吐量的 70%。 有关最终用户体验的信息，请参阅[更改OneDrive 同步应用上传或下载速率](https://support.office.com/article/71cc69da-2371-4981-8cc8-b4558bdda56e)。

 重要

如果启用或禁用此设置，然后将其改回 **“未配置”**，则最后一个配置仍然有效。 建议启用此设置，而不是 **将同步应用上传速度限制为固定速率** 以限制上传速率。 不应同时启用这两个设置。

### 防止自动进行身份验证

此设置确定同步客户端是否可以自动登录。

如果启用此设置，则会阻止同步使用向 Microsoft 应用程序提供的现有 Microsoft Azure Active Directory (Azure AD) 凭据自动登录。

如果禁用或未配置此设置，同步将自动登录。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"DisableAutoConfig"=dword:00000001`​

### 在用户登录前阻止同步应用生成网络流量

此设置允许阻止OneDrive 同步应用 (OneDrive.exe) 生成网络流量 (检查更新) ，直到用户登录到 OneDrive 或开始同步其计算机上的文件。

如果你启用此设置，用户必须先在其计算机上登录 OneDrive 同步应用，或选择同步计算机上的 OneDrive 或 SharePoint 文件，然后同步应用才能自动启动。

如果禁用或未配置此设置，当用户登录到 Windows 时，OneDrive 同步应用会自动启动。

 重要

如果启用或禁用此设置，然后将其改回 **“未配置”**，则最后一个配置仍然有效。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"PreventNetworkTrafficPreUserSignIn"=dword:00000001`​

### 阻止用户远程提取文件

 备注

已从 OneDrive 管理模板文件 （ADMX/ADML） 中删除此设置，因为获取文件功能于 2020 年 7 月 31 日弃用。

### 阻止用户将 Windows 已知文件夹移动到 OneDrive

此设置可阻止用户将“文档”、“图片”和“桌面”文件夹移到任何 OneDrive 帐户中。

 备注

在已加入域的电脑上，已阻止将已知文件夹移到个人 OneDrive 帐户。

如果启用此设置，用户不会看到保护重要文件夹的窗口提示，且“*管理备份*”命令处于禁用状态。 如果用户已移动其已知文件夹，则这些文件夹中的文件将保留在 OneDrive 中。 若要将已知文件夹重定向回用户的设备，请选择“ **否**”。 如果已启用提示用户将 Windows 已知文件夹移动到 OneDrive 或 **以静默方式将 Windows 已知文件夹移动到 OneDrive**，则此设置不会生效。

如果禁用或未配置此设置，用户可以选择移动其已知文件夹。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMBlockOptIn"=dword:00000001`​

若要将已知文件夹重定向到用户的设备并启用此策略，请设置以下注册表项值 2：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMBlockOptIn"=dword:00000002`​

### 阻止用户将其 Windows 已知文件夹重定向到电脑

此设置强制用户将“文档”、“图片”和“桌面”文件夹定向到 OneDrive。

如果启用此设置，则“**设置重要文件夹保护**”窗口中的“**停止保护**”按钮处于禁用状态，且用户会在尝试停止同步已知文件夹时看到错误消息。

如果禁用或未配置此设置，用户可以选择将其已知文件夹重定向回其电脑。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMBlockOptOut"=dword:00000001`​

### 防止用户同步从其他组织共享的库和文件夹

通过 OneDrive 同步应用的 B2B 同步功能，组织中的用户可同步其他组织与之共享的 OneDrive 和 SharePoint 库及文件夹。 有关详细信息，请参阅 [B2B 同步](https://learn.microsoft.com/zh-cn/sharepoint/b2b-sync)。

启用此设置会阻止组织中的用户使用 B2B 同步。在计算机上启用设置 (值 1) 后，同步应用不会同步从其他组织共享的库和文件夹。 将设置修改为禁用状态（值 0），以便为用户恢复 B2B 同步功能。

阻止与以下 B2B 同步：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive] "BlockExternalSync"=dword:1`​

恢复与以下 B2B 同步：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive] "BlockExternalSync"=dword:0`​

### 提示用户将 Windows 已知文件夹移动到 OneDrive

此设置显示以下窗口，以提示用户将“文档”、“图片”和“桌面”文件夹移到 OneDrive 中。

​![提示用户备份重要文件夹的窗口](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/kfm-wizard.png)​

如果启用此设置并提供租户 ID，要同步 OneDrive 的用户会在登录时看到上述窗口。 如果用户关闭此窗口，活动中心内会显示提醒通知，直到用户移动其全部已知文件夹为止。 如果用户已将其已知文件夹重定向到其他 OneDrive 帐户，系统会提示他们将文件夹定向到组织的帐户， (将现有文件留在) 后面。

如果禁用或未配置此设置，则不会显示提示用户保护其重要文件夹的窗口。

启用此策略会将以下注册表项设置为以下示例中显示的值：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMOptInWithWizard"="1111-2222-3333-4444"`​

（其中，“1111-2222-3333-4444”是[租户 ID](https://learn.microsoft.com/zh-cn/sharepoint/find-your-office-365-tenant-id)）。

有关信息和建议，请参阅 [重定向 Windows 已知文件夹并将其移动到 OneDrive](https://learn.microsoft.com/zh-cn/sharepoint/redirect-known-folders)。

### 当用户在其本地计算机上删除多个 OneDrive 文件时提示用户

此策略设置了用户可以从本地 OneDrive 文件夹中删除的文件数阈值，超过此阈值就会通知用户这些文件还会从云中删除。

在你启用此策略后，如果用户从本地计算机上的 OneDrive 文件夹中删除超过指定数量的文件，就会看到通知。 用户可以视需要选择继续删除云文件，还是还原本地文件。

 备注

即使启用此策略，如果用户选中了上一个通知上的“**始终删除文件**”复选框，或者他们取消选中了OneDrive 同步应用设置中的“在**云中删除许多文件时通知我**”复选框，用户也不会收到通知。

如果禁用此策略，当用户删除本地计算机上的大量 OneDrive 文件时，他们不会收到通知。

如果未配置此策略，当用户在短时间内删除超过 200 个文件时，将看到通知。

启用此策略将下面的注册表项值设置为介于 0 和 100000 之间的数字：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"LocalMassDeleteFileDeleteThreshold"`​

### 要求用户确认大型删除操作

此设置使用户确认他们想要在删除大量同步文件时删除云中的文件。

如果启用此设置，则在用户删除大量同步文件时会始终显示警告。 如果用户在 7 天内未确认删除操作，则不会删除文件。

如果禁用或未配置此设置，用户可以选择隐藏警告，并始终删除云中的文件。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"ForcedLocalMassDeleteDetection"=dword:00000001`​

### 设置可自动下载的用户 OneDrive 的最大大小

此设置与 [在未启用 OneDrive Files On-Demand 的设备上，使用其 Windows 凭据以无提示方式登录到 OneDrive 同步应用](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#silently-sign-in-users-to-the-onedrive-sync-app-with-their-windows-credentials)。 如果用户的 OneDrive 超过指定阈值（以 MB 为单位），系统会提示用户选择要同步的文件夹，然后 OneDrive 同步应用 (OneDrive.exe) 才会下载文件。

要输入租户 ID 和大小上限（0-4294967295，以 MB 为单位），请在“**选项**”框中选择“**显示**”。 默认值为 **500**。

启用此策略会设置下面的注册表项：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive\DiskSpaceCheckThresholdMB]"1111-2222-3333-4444"=dword:0005000`​

其中，“1111-2222-3333-4444”是 [租户 ID](https://learn.microsoft.com/zh-cn/sharepoint/find-your-office-365-tenant-id) ，而“0005000”将阈值设置为“5000 MB”。

### 设置同步应用更新通道

我们通过三个通道向公众发布了 OneDrive 同步应用 (OneDrive.exe) 更新：第一个是预览体验成员通道，然后是生产通道，最后是“已推迟”通道。 此设置使你能够为组织中的用户指定圈。 启用此设置并选择环时，用户无法更改它。

预览体验计划通道用户会收到让其能够预览 OneDrive 中新功能的版本。

生产通道用户会在最新功能可用时获得这些功能。 此圈是默认的。

延期圈用户将获取新功能、错误修复和新的性能改进。 借助此通道，可从内部网络位置部署更新并控制部署时间（限于 60 天内）。

 重要

我们建议选择 IT 部门中的若干人员作为早期采用者，以加入预览体验通道，并尽早接收功能。 建议将组织中所有其他人置于默认的生产通道中，确保他们及时收到 漏洞修补程序和新功能。 [请参阅有关配置同步应用的所有建议。](https://learn.microsoft.com/zh-cn/sharepoint/ideal-state-configuration)

如果禁用或未配置此设置，用户可以加入 [Windows 预览体验计划](https://insider.windows.com/) 或 [Office 预览体验计划](https://products.office.com/office-insider) 以获取预览体验成员圈上的更新。

启用此策略会设置下面的注册表项：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"GPOSetUpdateRing"=dword:0000000X`​

为预览体验成员设置值 **4** ，为生产设置 **值 5** ，或者为延期设置 **值 0** 。 将此设置配置为“生产”的**“5**”或“延迟”的“**0**”时，同步应用中的“在发布前获取 OneDrive 预览体验成员预览版更新”，“**关于设置”&gt;**选项卡上不会显示复选框。

有关每个环中当前可用的版本的详细信息，请参阅 [发行说明](https://support.office.com/article/845dcf18-f921-435e-bf28-4e24b95e5fc0?)。 有关更新通道以及同步应用如何检查更新的详细信息，请参阅 [OneDrive 同步 应用更新过程](https://learn.microsoft.com/zh-cn/sharepoint/sync-client-update-process)。

### 将 Windows 已知文件夹以无提示方式移动到 OneDrive

使用此设置，无需任何用户交互，即可将用户的“文档”、“图片”和/或“桌面”文件夹重定向并移动到 OneDrive。

 备注

建议为现有设备和新设备部署无提示策略，同时将现有设备的部署限制为每天 1,000 台设备，每周不超过 4,000 台设备。 还建议结合使用此设置和[提示用户将 Windows 已知文件夹移动到 OneDrive](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#prompt-users-to-move-windows-known-folders-to-onedrive)。 如果以无提示方式移动已知文件夹不成功，系统会提示用户更正错误并继续。 [请参阅有关配置同步应用的所有建议。](https://learn.microsoft.com/zh-cn/sharepoint/ideal-state-configuration)

可以一次移动所有文件夹，也可以选择要移动的文件夹。 移动文件夹后，即使取消选中文件夹的复选框，此策略也不会再次影响该文件夹。

如果你启用此设置并提供租户 ID，可选择是否在重定向文件夹后向用户显示通知。

​![OneDrive 保护消息](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/d28dbca8-f51a-43b2-b069-c483a53c6d0b.png)​

如果禁用或未配置此设置，则用户的已知文件夹不会以无提示方式重定向到 OneDrive。

启用此策略会设置下面的注册表项：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMSilentOptIn"="1111-2222-3333-4444"`​

其中“1111-2222-3333-4444”是表示 [租户 ID](https://learn.microsoft.com/zh-cn/sharepoint/find-your-office-365-tenant-id) 的字符串值。

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMSilentOptInWithNotification"=dword:00000001`​

将此值设置为 **1** 会显示成功重定向后的通知。

如果未设置以下任何策略，则默认策略会将桌面、文档和图片) (的所有文件夹移动到 OneDrive 中。 如果要指定要移动的文件夹 () ，可以设置以下策略的任意组合：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMSilentOptInDesktop"=dword:00000001`​：将此值设置为 **1** 将移动 Desktop 文件夹。

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMSilentOptInDocuments"=dword:00000001`​：将此值设置为 **1** 将移动 Documents 文件夹。

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"KFMSilentOptInPictures"=dword:00000001`​：将此值设置为 **1** 将移动“图片”文件夹。

有关详细信息，请参阅 [重定向 Windows 已知文件夹并将其移动到 OneDrive](https://learn.microsoft.com/zh-cn/sharepoint/redirect-known-folders)。

### 让客户使用 Windows 凭据以无提示方式登录 OneDrive 同步应用

 重要

通过 `SilentAccountConfig`​预配同步用户时，[会自动启用 Azure Active Directory 身份验证库](https://learn.microsoft.com/zh-cn/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) ;因此无需单独启用它。

*使用 Windows 凭据以静默方式将用户登录到 OneDrive 同步 应用*功能适用于已加入 Azure Active Directory (Azure AD) 的计算机。

如果启用此设置，在已加入 Azure AD 的计算机上登录的用户无需输入其帐户凭据即可设置其同步应用。 用户仍会显示 OneDrive 安装程序，以便他们可以选择要同步和更改其 OneDrive 文件夹位置的文件夹。 如果用户使用的是先前的 OneDrive for Business 同步应用 (Groove.exe)，则新的同步应用将尝试接管先前应用上的用户 OneDrive 同步任务，并保留用户的同步设置。 此设置经常与“[设置可自动下载的用户 OneDrive 的最大大小](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#set-the-maximum-size-of-a-users-onedrive-that-can-download-automatically)”（在未启用文件随选的电脑上）和“[设置 OneDrive 文件夹的默认位置](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#set-the-default-location-for-the-onedrive-folder)”搭配使用。

 重要

建议在配置同步应用时启用 "静默帐户配置"。 [请参阅有关配置同步应用的所有建议。](https://learn.microsoft.com/zh-cn/sharepoint/ideal-state-configuration)

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"SilentAccountConfig"=dword:00000001`​

有关此功能的详细信息（包括故障排除步骤），请参阅 [静默配置用户帐户](https://learn.microsoft.com/zh-cn/sharepoint/use-silent-account-configuration)。 如果你对此功能有反馈或遇到任何问题，请告诉我们。 先右键单击通知区域中的 OneDrive 图标，再选择“**报告问题**”。 请对任何反馈添加“SilentConfig”标记，这样反馈就会直接发送给负责此功能的工程师。

### 指定 SharePoint Server URL 和组织名称

此设置适用于具有 SharePoint Server 2019 的客户。 有关将新的 OneDrive 同步 应用与SharePoint Server 2019配合使用的信息，请参阅[配置与新OneDrive 同步应用的同步](https://learn.microsoft.com/zh-cn/SharePoint/install/configure-syncing-with-the-onedrive-sync-app/)。

### 在混合环境中指定 OneDrive 位置

此设置适用于具有 SharePoint Server 2019 的客户。 有关将新的 OneDrive 同步 应用与SharePoint Server 2019配合使用的信息，请参阅[配置与新OneDrive 同步应用的同步](https://learn.microsoft.com/zh-cn/SharePoint/install/configure-syncing-with-the-onedrive-sync-app/)。

### 使用 OneDrive 文件随选

使用此设置，可控制是否对组织启用 OneDrive 文件随选。 文件按需有助于节省用户计算机上的存储空间，并最大程度地减少同步对网络的影响。此功能适用于运行 Windows 10 Fall Creators 更新 (版本 1709 或更高版本) 的用户。 更多相关信息，请参阅 [通过适用于 Windows 10 的 OneDrive 文件按需节省磁盘空间](https://support.office.com/article/0e6860d3-d9f3-4971-b321-7092438fb38e)。

 重要

建议保留文件按需启用。 [请参阅有关配置同步应用的所有建议。](https://learn.microsoft.com/zh-cn/sharepoint/ideal-state-configuration)

如果你启用此设置，则设置同步应用的新用户会默认在文件资源管理器中看到仅联机文件。 打开文件前，不会下载文件内容。 如果禁用此设置，Windows 10 用户的同步行为会与旧版 Windows 用户相同，并且无法启用文件随选。 如果未配置此设置，用户可以打开或关闭“文件按需”选项。

启用此策略会将下面的注册表项值设置为 1：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive]"FilesOnDemandEnabled"=dword:00000001`​

符合 Windows 和 OneDrive 同步应用要求，但“设置”中仍未显示“文件随选”选项？ 确保服务“Windows 云文件筛选器驱动程序”启动类型设置为 **2** (AUTO_START) 。 启用此功能会将下面的注册表项值设置为 2：

​`[HKLM\SYSTEM\CurrentControlSet\Services\CldFlt]"Start"=dword:00000002`​

### 向磁盘空间不足的用户发出警告

通过此设置，可指定最小的可用磁盘空间量，并在 OneDrive 同步应用 (OneDrive.exe) 下载会导致用户的磁盘容量小于此数量的文件时向用户发出警告。 系统会提示用户选择相应的选项以帮助释放空间。

启用此策略会将下面的注册表项值设置为介于 0 和 10240000 之间的数字：

​`[HKLM\SOFTWARE\Policies\Microsoft\OneDrive] "WarningMinDiskSpaceLimitInMB"=dword:00000000`​

## 用户配置策略

*用户配置策略*位于 User Configuration\Policies\Administrative Templates\OneDrive 下。

​![组策略管理编辑器中的 OneDrive 设置](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/8e121823-b5bf-440c-999d-c2a9ada4705d.png)​

### 允许用户选择如何处理 Office 文件同步冲突

此设置指定在同步期间 Office 文件版本之间发生冲突时发生的情况。 (此选项仅适用于 Office 2016 或更高版本。使用早期版本的 Office 时，始终保留这两个副本。)

如果你启用此设置，用户可决定是要合并更改，还是要保留两个副本。

启用此策略会将下面的注册表项值设置为 1：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive] "EnableHoldTheFile"=dword:00000001`​

如果禁用此设置，则当发生同步冲突时，将保留文件的两个副本。

要启用此设置，必须启用“[在 Office 桌面应用中合著和共享](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#coauthor-and-share-in-office-desktop-apps)”。

### 在 Office 桌面应用中合著和共享

此设置让多个用户可以使用 Microsoft 365 企业应用版、Office 2019 或 Office 2016 桌面应用来同时编辑 OneDrive 中存储的 Office 文件。 它还可以让用户通过 Office 桌面应用共享文件。

 重要

建议保留启用此设置，使同步速度更快，并减少网络带宽。 [请参阅有关配置同步应用的所有建议。](https://learn.microsoft.com/zh-cn/sharepoint/ideal-state-configuration)

启用此策略会将下面的注册表项值设置为 1：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive] "EnableAllOcsiClients"=dword:00000001`​

如果禁用此设置，将禁用 Office 文件的共同创作和应用内共享。 发生文件冲突时，将保留文件的两个副本。

### 配置要自动同步的团队网站库

通过此设置，可指定 SharePoint 团队网站库在用户下次登录到 OneDrive 同步应用 (OneDrive.exe) 后的 8 小时时段内自动同步以帮助分配网络负载。 要使用此设置，计算机必须运行 Windows 10 Fall Creators Update（版本 1709）或更高版本，且必须启用 OneDrive 文件随选。 未为本地 SharePoint 网站启用此功能。

 重要

出于性能原因，建议不要对具有超过 5,000 个文件或文件夹的库启用此设置。 不要对具有 1，000 多个设备的同一库启用此设置。

如果启用此设置，则 OneDrive 同步应用会在用户下次登录时自动同步你指定为仅联机文件的库的内容。 用户无法停止同步库。

如果禁用此设置，则对于新用户来说，不会自动同步未指定的团队网站库。 现有用户可以选择停止同步库，但库不会自动停止同步。

若要配置设置，请在“**选项**”框中选择“**显示**”，在**“值名称”**字段中输入一个易记名称来标识库，然后在**“值**”字段中 (tenantId=xxx siteId=xxx&webId=xxx&&listId=xxx&webUrl=httpsxxx&version=1) 。

若要查找库 ID，请在 Microsoft 365 中以全局或 SharePoint 管理员身份登录，浏览至该库，然后选择“**同步**”按钮。在“**开始同步**”对话框中，选择“**复制库 ID**”链接。

​![“同步准备就绪”对话框](https://learn.microsoft.com/zh-cn/sharepoint/sharepointonline/media/copy-library-id.png)​

此复制字符串中的特殊字符采用 Unicode，并且必须根据下表将其转换为 ASCII。

|查找|替换|
| ------| ------|
|%2D|-|
|%7B|{|
|%7D|}|
|%3A|:|
|%2F|/|
|%2E|.|

或者，可以在 PowerShell 中运行以下命令，将"复制的字符串"替换为库 ID：

**PowerShell**复制

```
[uri]::UnescapeDataString("Copied String")
```

启用此策略将会设置以下注册表项，以使用复制的库的完整 URL：

​`[HKCU\Software\Policies\Microsoft\OneDrive\TenantAutoMount]"LibraryName"="LibraryID"`​

### 在使用按流量计费的网络时继续同步

此设置使你能够在设备连接至计费网络时关闭自动暂停功能。

如果启用此设置，则在设备位于计费网络上时会继续同步。 OneDrive 不会自动暂停同步。

如果禁用或未配置此设置，同步会在检测到按流量计费的网络时自动暂停，并显示通知。 若要不暂停，请在通知中选择“继续同步”。 如果同步已暂停，若要恢复同步，请选择任务栏通知区域中的 OneDrive 云图标，然后选择活动中心顶部的警报。

启用此策略会将下面的注册表项值设置为 1：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive] "DisablePauseOnMeteredNetwork"=dword:00000001`​

### 在设备打开省电模式时继续同步

此设置使你能够为已打开节电模式的设备关闭自动暂停功能。

如果启用此设置，则在用户打开节电模式时会继续同步。 OneDrive 不会自动暂停同步。

如果禁用或未配置此设置，则检测到节电模式时，同步会自动暂停，并显示通知。 若要不暂停，请在通知中选择“继续同步”。 如果同步已暂停，若要恢复同步，请选择任务栏通知区域中的 OneDrive 云图标，然后选择活动中心顶部的警报。

启用此策略会将下面的注册表项值设置为 1：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive] "DisablePauseOnBatterySaver"=dword:00000001`​

### 禁用在 OneDrive 安装程序结束时显示的教程

此设置使你能够阻止在 OneDrive 安装结束时显示教程。

如果启用此设置，则用户在完成 OneDrive 安装后不会看到教程。

启用此策略会将下面的注册表项值设置为 1：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive] "DisableTutorial"=dword:00000001`​

### 将同步应用下载速度限制为固定速率

此设置使你能够配置 OneDrive 同步应用 (OneDrive.exe) 可达到的最大文件下载速度。 此速率为固定值，单位为 KB/秒，仅适用于同步，不适用于下载更新。 速率越低，文件下载速度越慢。

建议在文件随选未启用且必须遵守严格流量限制的情况下（如最初在组织中部署同步应用或启用同步团队网站时）使用此设置。 建议不要持续使用此设置，因为这会降低同步应用性能，并对用户体验造成负面影响。 初始同步后，用户通常一次只同步几个文件，这对网络性能不会造成显著影响。 如果启用此设置，计算机将使用指定的最大下载速率，用户无法更改它。

如果启用此设置，请在“**带宽**”框中输入速率（1 到 100000）。 最大速率为 100000 KB/s。 任何低于 50 KB/s 的输入均会将限制设为 50 KB/s，即便 UI 上显示更低的值也是如此。

如果禁用或未配置此设置，则下载速率不受限制，用户可以选择在OneDrive 同步应用设置中对其进行限制。 有关最终用户体验的信息，请参阅[更改OneDrive 同步应用上传或下载速率](https://support.office.com/article/71cc69da-2371-4981-8cc8-b4558bdda56e)。

启用此策略会将下面的注册表项值设置为介于 50 和 100,000 之间的数字。 例如：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive] "DownloadBandwidthLimit"=dword:00000032`​

上述注册表项使用 50 对应的十六进制值（即 00000032），将下载吞吐率限制设置为 50KB/s。

 备注

必须在用户计算机上重新启动 OneDrive.exe 才能应用此设置。

有关估算同步所需的网络带宽的信息，请参阅 [OneDrive 同步 应用的网络利用率规划](https://learn.microsoft.com/zh-cn/sharepoint/network-utilization-planning)。

### 将同步应用上传速度限制为固定速率

此设置使你能够配置 OneDrive 同步应用 (OneDrive.exe) 可达到的最大文件上传速度。 此速率为固定值，单位为 KB/秒。 速率越低，计算机上传文件的速度越慢。

如果启用此设置并在“ **带宽** ”框中输入 (从 1 到 100000) 的速率，则计算机将使用你指定的最大上传速率，并且用户无法在 OneDrive 设置中更改它。 最大速率为 100000 KB/s。 任何低于 50 KB/s 的输入均会将限制设为 50 KB/s，即便 UI 上显示更低的值也是如此。

如果禁用或未配置此设置，用户可以选择将上传速率限制为固定值 (KB/秒) ，或将其设置为 **“自动调整”** ，将上传速率设置为吞吐量的 70%。 有关最终用户体验的信息，请参阅[更改OneDrive 同步应用上传或下载速率](https://support.office.com/article/71cc69da-2371-4981-8cc8-b4558bdda56e)。

建议仅在必须遵守严格流量限制的情况下才使用此设置。 如果需要限制上传速率（例如，在部署已知文件夹迁移时），建议使用“[将同步应用上传速率限制为吞吐量的一定百分比](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#limit-the-sync-app-upload-rate-to-a-percentage-of-throughput)”，以便设定的限制适应不断变化的情况。 请勿同时启用这两个设置。

启用此策略会将以下注册表项值设置为 50 到 100，000 的数字：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive]"UploadBandwidthLimit"=dword:00000032`​

上述注册表项使用 50 对应的十六进制值（即 00000032），将上传吞吐率限制设置为 50KB/s。

 备注

必须在用户计算机上重新启动 OneDrive.exe 才能应用此设置。

有关估算同步所需的网络带宽的信息，请参阅 [OneDrive 同步 应用的网络利用率规划](https://learn.microsoft.com/zh-cn/sharepoint/network-utilization-planning)。

### 阻止用户更改其 OneDrive 文件夹的位置

此设置使你能够阻止用户更改 OneDrive 文件夹在其计算机上的位置。

若要使用此设置，请在“**选项**”框中选择“**显示**”，然后输入你的“[租户 ID](https://learn.microsoft.com/zh-cn/sharepoint/find-your-office-365-tenant-id)”。 若要启用设置，请输入 **1**;若要禁用它，请输入 **0**。

如果启用此设置，则 OneDrive 安装程序中的“**更改位置**”链接将会隐藏。 如果启用“[设置 OneDrive 文件夹的默认位置](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#set-the-default-location-for-the-onedrive-folder)”，则会在默认位置或指定的自定义位置创建 OneDrive 文件夹。

启用此策略会将下面的注册表项值设置为 1：

​`[HKCU\Software\Policies\Microsoft\OneDrive\DisableCustomRoot] "1111-2222-3333-4444"=dword:00000001`​

其中，”1111-2222-3333-4444” 是租户 ID。

如果禁用此设置，用户可以在 OneDrive 安装程序中更改其同步文件夹的位置。

### 阻止用户同步个人 OneDrive 帐户

此设置使你能够阻止用户使用 Microsoft 帐户登录来同步其个人 OneDrive 文件。 默认允许用户同步个人 OneDrive 帐户。

如果启用此设置，则会阻止用户设置其个人 OneDrive 帐户的同步关系。 启用此设置时，已同步其个人 OneDrive 的用户无法继续同步， (他们收到同步已停止) 的消息，但同步到计算机的任何文件都保留在计算机上。

启用此策略会将下面的注册表项值设置为 1：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive]"DisablePersonalSync"=dword:00000001`​

### 在延期圈上接收 OneDrive 同步应用更新

 重要

此设置很快将被删除。 我们建议使用新的设置“[设置同步应用更新通道](https://learn.microsoft.com/zh-cn/sharepoint/use-group-policy#set-the-sync-app-update-ring)”。

有关更新通道以及同步应用如何检查更新的详细信息，请参阅 [OneDrive 同步 应用更新过程](https://learn.microsoft.com/zh-cn/sharepoint/sync-client-update-process)。

### 设置 OneDrive 文件夹的默认位置

此设置使你能够将指定路径设为 OneDrive 文件夹在用户计算机上的默认位置。 默认情况下，此路径位于 %userprofile% 下。

如果启用此设置，则 *OneDrive - {organization name}* 文件夹的默认位置是指定的路径。 要指定租户 ID 和路径，请在“**选项**”框中选择“**显示**”。

此策略将下面的注册表项设置为指定文件路径的字符串：

​`[HKCU\SOFTWARE\Policies\Microsoft\OneDrive\DefaultRootDir] "1111-2222-3333-4444"="{User path}"`​

其中，”1111-2222-3333-4444” 是租户 ID。

如果禁用此设置，则本地 *OneDrive - {organization name}* 文件夹位置默认为 %userprofile%。

 备注

无法通过组策略使用 %logonuser% 环境变量。 建议改用 %username%。
