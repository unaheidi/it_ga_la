### 前言

除了向 GitLab.com 反馈Bug和改进建议外，如果能向该项目贡献代码，就更有意义了！  
看了[官网说明](https://docs.gitlab.com/ce/development/)后，了解到提交变更必须配上自动化测试的相关代码。  
本文主要关注 GitLab 用到的:  
* 用于后端测试的[RSpec](https://github.com/rspec/rspec-rails/)
* 用于端到端集成测试的[Capybara](https://github.com/teamcapybara/capybara)

希望把上手这两个测试工具的过程做个记录：
* 一来了解了DSL的来历后可以熟练使用这些功能。
* 二来记录如何上手新的工具，以便激励自己继续前行。


