# 贡献指南 | Contributing Guide

感谢您对微信公众号数据抓取工具项目的关注！我们欢迎任何形式的贡献，包括但不限于：

- 🐛 Bug 报告和修复
- ✨ 新功能建议和实现  
- 📚 文档改进和翻译
- 🧪 测试用例编写
- 💡 性能优化建议
- 🔍 代码审查和反馈

## 📋 贡献前须知

### 法律合规声明
在开始贡献之前，请确保您理解并同意：

- **合规使用**: 本项目仅用于学习研究目的，禁止用于商业用途
- **法律责任**: 贡献者和使用者需自行承担法律责任
- **平台规则**: 严格遵守各平台的服务条款和使用协议
- **数据保护**: 尊重用户隐私，不得滥用获取的数据

### 技术要求
- Python 3.7+
- 熟悉 Git 版本控制
- 了解网络爬虫基本原理
- 具备基本的 Web 开发知识

## 🚀 快速开始

### 1. 环境搭建

```bash
# Fork 并克隆仓库
git clone https://github.com/yourusername/Wechat_Public_article.git
cd Wechat_Public_article

# 创建虚拟环境
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate  # Windows

# 安装依赖
pip install -r requirements.txt
pip install -r requirements-dev.txt

# 配置环境变量
cp .env.example .env
# 编辑 .env 文件，填入必要配置
```

### 2. 运行测试

```bash
# 运行所有测试
pytest

# 运行特定测试
pytest tests/unit/test_crawler.py

# 生成测试覆盖率报告
pytest --cov=src --cov-report=html
```

### 3. 代码格式检查

```bash
# 代码格式化
black src/ tests/ examples/

# 导入排序
isort src/ tests/ examples/

# 语法检查
flake8 src/ tests/

# 类型检查
mypy src/
```

## 📝 贡献流程

### Step 1: Fork 和分支创建

```bash
# Fork 原仓库到您的账号下

# 克隆您的 Fork
git clone https://github.com/yourusername/Wechat_Public_article.git

# 添加上游仓库
git remote add upstream https://github.com/originalowner/Wechat_Public_article.git

# 创建功能分支
git checkout -b feature/your-feature-name
```

### Step 2: 开发和测试

```bash
# 进行您的修改...

# 运行测试确保没有破坏现有功能
pytest

# 添加新的测试用例（如果适用）
# 确保代码覆盖率不降低
```

### Step 3: 提交和推送

```bash
# 添加修改的文件
git add .

# 提交修改（使用清晰的提交信息）
git commit -m "feat: add new feature for account discovery"

# 推送到您的 Fork
git push origin feature/your-feature-name
```

### Step 4: 创建 Pull Request

1. 前往您的 Fork 页面
2. 点击 "New Pull Request"
3. 选择正确的分支
4. 填写详细的 PR 描述
5. 等待代码审查

## 📏 代码规范

### 提交信息规范

使用 [Conventional Commits](https://www.conventionalcommits.org/) 格式：

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**类型 (type)**:
- `feat`: 新功能
- `fix`: Bug 修复  
- `docs`: 文档更新
- `style`: 代码格式调整
- `refactor`: 代码重构
- `test`: 测试相关
- `chore`: 构建工具或依赖更新

**示例**:
```
feat(crawler): add support for video channel data extraction

- Implement new parser for video content
- Add configuration options for video crawling
- Update documentation with usage examples

Closes #123
```

### Python 代码规范

遵循 [PEP 8](https://pep8.org/) 和项目特定规范：

```python
# 好的示例
class WeChatCrawler:
    """微信公众号爬虫主类.
    
    Args:
        config: 配置对象
        proxy_pool: 代理池实例
    """
    
    def __init__(self, config: CrawlerConfig, proxy_pool: ProxyPool = None):
        self.config = config
        self.proxy_pool = proxy_pool or ProxyPool()
        self._session = None
        
    async def crawl_account(self, biz: str) -> CrawlResult:
        """抓取指定公众号数据.
        
        Args:
            biz: 公众号 biz 标识符
            
        Returns:
            CrawlResult: 抓取结果对象
            
        Raises:
            CrawlerError: 抓取失败时抛出
        """
        if not biz:
            raise ValueError("biz parameter cannot be empty")
            
        try:
            # 实现细节...
            pass
        except Exception as e:
            logger.error(f"Failed to crawl account {biz}: {e}")
            raise CrawlerError(f"Crawl failed: {e}") from e
```

### 文档规范

- 使用中英文双语注释（重要部分）
- API 文档使用 docstring 格式
- README 和技术文档使用 Markdown
- 代码示例要可运行

## 🧪 测试要求

### 测试覆盖率
- 新代码测试覆盖率不低于 80%
- 核心功能测试覆盖率不低于 90%
- 所有 public API 必须有测试

### 测试类型

**单元测试**:
```python
import pytest
from unittest.mock import Mock, patch

from wechat_crawler.core.crawler import WeChatCrawler

class TestWeChatCrawler:
    @pytest.fixture
    def crawler(self):
        config = Mock()
        return WeChatCrawler(config)
        
    @patch('wechat_crawler.core.crawler.requests.get')
    def test_fetch_article_success(self, mock_get, crawler):
        # 模拟成功请求
        mock_response = Mock()
        mock_response.status_code = 200
        mock_response.text = "<html>test content</html>"
        mock_get.return_value = mock_response
        
        result = crawler.fetch_article("test_url")
        
        assert result is not None
        assert "test content" in result.content
```

**集成测试**:
```python
import pytest
from fastapi.testclient import TestClient

from wechat_crawler.api.app import app

@pytest.fixture
def client():
    return TestClient(app)

def test_get_articles_endpoint(client):
    response = client.get("/api/v1/articles?biz=test_biz")
    
    assert response.status_code == 200
    data = response.json()
    assert "articles" in data
```

## 🔍 代码审查

### 审查要点
- **功能正确性**: 代码是否实现了预期功能
- **性能影响**: 是否存在性能问题
- **安全考虑**: 是否存在安全漏洞
- **代码质量**: 代码是否清晰、可维护
- **测试完整性**: 测试是否充分覆盖新代码
- **文档完整性**: 是否包含必要的文档更新

### 审查流程
1. **自动检查**: CI/CD 管道自动运行测试和代码检查
2. **人工审查**: 至少需要一名维护者的审查通过
3. **讨论和修改**: 根据反馈进行必要的修改
4. **最终合并**: 审查通过后合并到主分支

## 🐛 Bug 报告

### 报告格式

使用以下模板报告 Bug：

```markdown
## Bug 描述
简要描述遇到的问题

## 复现步骤
1. 执行操作 A
2. 设置参数 B
3. 观察结果 C

## 期望结果
描述您期望看到的结果

## 实际结果
描述实际发生的情况

## 环境信息
- 操作系统: [e.g. Ubuntu 20.04]
- Python 版本: [e.g. 3.9.7]
- 项目版本: [e.g. 1.0.0]
- 相关依赖版本: [e.g. requests==2.28.1]

## 附加信息
- 错误日志
- 截图（如适用）
- 配置文件内容（去除敏感信息）
```

### Bug 优先级
- **Critical**: 系统崩溃、数据丢失
- **High**: 核心功能不可用
- **Medium**: 功能异常但有替代方案
- **Low**: 小问题或改进建议

## 💡 功能请求

### 请求格式

```markdown
## 功能描述
清晰描述您希望添加的功能

## 使用场景
说明什么情况下需要这个功能

## 解决方案
如果有的话，描述您建议的实现方案

## 替代方案
描述您考虑过的其他解决方案

## 附加说明
任何其他相关信息
```

## 📚 文档贡献

### 文档类型
- **API 文档**: 接口说明和使用示例
- **教程文档**: 逐步操作指南
- **架构文档**: 系统设计和技术决策
- **部署文档**: 安装配置和运维指南

### 文档规范
- 使用清晰的标题和结构
- 提供实际可运行的代码示例
- 包含必要的图表和流程图
- 保持内容的及时更新

## 🏆 贡献者认可

### 贡献统计
我们会在以下地方认可贡献者：
- `CONTRIBUTORS.md` 文件
- 项目 README
- 版本发布说明
- 年度贡献者报告

### 贡献类型
- 💻 代码贡献
- 📖 文档改进
- 🐛 Bug 报告
- 💡 功能建议
- 🌍 翻译工作
- 🎨 设计改进
- 🧪 测试用例
- 📢 推广宣传

## ❓ 获取帮助

如果您在贡献过程中遇到问题，可以通过以下方式获取帮助：

- **GitHub Issues**: 技术问题和 Bug 报告
- **GitHub Discussions**: 功能讨论和一般问题
- **Email**: [项目邮箱] (紧急问题)
- **文档**: 查看项目文档获取详细信息

## 📄 许可协议

通过向本项目贡献代码，您同意您的贡献将在 [MIT 许可证](LICENSE) 下发布。

---

再次感谢您的贡献！每一份贡献都让这个项目变得更好。🙏
