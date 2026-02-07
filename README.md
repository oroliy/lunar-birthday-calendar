# 农历生日日历生成器 / Lunar Birthday Calendar Generator

一个简单易用的农历生日日历生成器，将农历生日转换为可导入主流日历应用的ICS文件。

A simple and easy-to-use lunar birthday calendar generator that converts lunar birthdays into ICS files that can be imported into mainstream calendar applications.

## 功能特性 / Features

- ✅ 添加农历生日（支持姓名和备注）
- ✅ 选择农历日期（月份、日期、闰月支持）
- ✅ 自动计算下一年公历日期预览
- ✅ 生成永久重复的ICS日历文件
- ✅ 本地数据持久化（localStorage）
- ✅ 简繁体中文切换
- ✅ 响应式设计，支持移动端和桌面端
- ✅ 纯静态网页，无需服务器

## 在线使用 / Online Usage

访问在线版本即可直接使用：https://oroliy.github.io/lunar-birthday-calendar/

## 本地使用 / Local Usage

### 安装 / Installation

1. 克隆仓库：
```bash
git clone https://github.com/oroliy/lunar-birthday-calendar.git
cd lunar-birthday-calendar
```

2. 直接在浏览器中打开 `index.html` 文件

无需安装任何依赖或运行任何构建命令！

### 使用方法 / Usage

1. 在"添加农历生日"表单中输入姓名或备注
2. 选择参考年份（用于判断是否有闰月）
3. 选择农历月份和日期
4. 如果是闰月，勾选"闰月"选项
5. 可选：添加备注（例如：爸爸的生日）
6. 点击"添加生日"按钮
7. 可以继续添加更多生日
8. 点击"生成ICS文件"按钮下载日历文件
9. 将下载的 `.ics` 文件导入到您的日历应用中

### 导入ICS文件 / Importing ICS Files

生成ICS文件后，请根据您使用的日历应用导入：

#### Google Calendar
1. 打开 Google Calendar
2. 点击设置图标（齿轮）
3. 选择"导入和导出"
4. 选择"从文件导入"
5. 选择下载的 `.ics` 文件并导入

#### Apple Calendar (macOS/iOS)
1. 双击下载的 `.ics` 文件
2. 或在 Apple Calendar 中选择"文件" > "导入"
3. 选择 `.ics` 文件并导入

#### Outlook
1. 打开 Outlook
2. 选择"文件" > "打开和导出"
3. 选择"打开日历"
4. 选择下载的 `.ics` 文件并导入

#### 其他日历应用

大多数日历应用都支持ICS文件格式，请参考相应应用的导入说明。

## 技术栈 / Tech Stack

- **纯HTML/CSS/JavaScript** - 无需构建工具
- **lunar-javascript v1.7.7** - 农历/公历转换
- **chinese-conv v4.0.0** - 简繁体转换
- **ics.js** - ICS文件生成
- **CDN加载** - 使用jsDelivr CDN加载依赖库

## 注意事项 / Important Notes

1. **闰月处理**
   - 农历闰月不固定，每年不同
   - 需要用户手动选择"闰月"选项
   - 系统会根据选择的参考年份验证闰月是否存在
   - 如果选择的年份没有闰月，系统会显示警告

2. **ICS文件格式**
   - 生成的ICS文件使用全年重复规则（YEARLY）
   - 事件为全天事件
   - 永久重复（无结束日期）
   - 兼容Google Calendar、Apple Calendar、Outlook等主流日历应用

3. **数据安全**
   - 所有数据保存在浏览器本地（localStorage）
   - 不会上传到任何服务器
   - 清除浏览器数据会丢失所有保存的生日信息

4. **浏览器兼容性**
   - 建议使用现代浏览器（Chrome、Firefox、Safari、Edge）
   - 需要启用JavaScript
   - 需要支持localStorage

5. **时区**
   - ICS文件使用UTC时间
   - 全天事件不受时区影响
   - 日历应用会自动处理时区转换

## 项目结构 / Project Structure

```
lunar-birthday-calendar/
├── index.html          # 主应用程序文件
├── LICENSE            # MIT许可证
├── README.md          # 项目文档
└── .github/
    └── workflows/
        └── deploy.yml  # GitHub Pages自动部署配置
```

## 部署 / Deployment

本项目已自动部署到GitHub Pages：https://oroliy.github.io/lunar-birthday-calendar/

### 自定义部署 / Custom Deployment

可以使用任何静态网站托管服务部署：

1. **GitHub Pages**
   - 推送代码到GitHub仓库
   - 在仓库设置中启用GitHub Pages
   - 选择main分支作为源

2. **Netlify**
   ```bash
   npm install -g netlify-cli
   netlify deploy --prod
   ```

3. **Vercel**
   ```bash
   npm install -g vercel
   vercel deploy
   ```

4. **其他静态托管**
   - AWS S3 + CloudFront
   - Cloudflare Pages
   - 任何支持静态网站的托管服务

## 贡献 / Contributing

欢迎提交问题和拉取请求！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建拉取请求

## 许可证 / License

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 联系方式 / Contact

- 作者: oroliy
- 仓库: https://github.com/oroliy/lunar-birthday-calendar
- 问题反馈: https://github.com/oroliy/lunar-birthday-calendar/issues

## 致谢 / Acknowledgments

感谢以下开源项目：

- [lunar-javascript](https://6tail.cn/lunar/) - 农历/公历转换库
- [ics.js](https://github.com/nwcell/ics.js) - ICS文件生成库
- [chinese-conv](https://github.com/pleasurazy/chinese-conv) - 简繁体转换库

## 常见问题 / FAQ

**Q: 闰月是什么意思？**

A: 农历为了调整与太阳年（回归年）的长度，每隔几年就会增加一个闰月。闰月可能出现在任何月份，不是固定的。例如，某年可能有"闰五月"，意思是五月有两个，一个正常五月，一个闰五月。

**Q: 如何确定我的生日是闰月还是普通月？**

A: 您可以查询农历日历或咨询家人确认。如果不确定，可以分别添加普通月和闰月两个版本，下一年公历日期会告诉您哪个是正确的。

**Q: 生成的ICS文件可以编辑吗？**

A: 可以的。ICS文件是纯文本文件，您可以用文本编辑器打开和编辑。但建议使用本工具重新生成以避免格式错误。

**Q: 支持批量导入吗？**

A: 目前不支持批量导入，需要逐个添加。未来版本可能会支持CSV等格式的批量导入。

**Q: 数据会丢失吗？**

A: 
- 清除浏览器缓存/数据不会丢失localStorage数据
- 但在不同浏览器或设备间不会自动同步
- 建议定期导出ICS文件备份

**Q: 支持英文界面吗？**

A: 目前仅支持简体中文和繁体中文。未来版本可能会添加英文支持。

---

如果这个项目对您有帮助，请给个 ⭐️ Star！
