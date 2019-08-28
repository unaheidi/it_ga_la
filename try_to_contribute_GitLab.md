### 序言

除了向 GitLab.com 反馈Bug和改进建议外，如果能向该项目贡献代码，就更有意义了！  
看了[官网说明](https://docs.gitlab.com/ce/development/)后，了解到提交变更必须配上自动化测试的相关代码。  
本文主要关注 GitLab 用到的:  
* 用于后端测试的[RSpec](https://github.com/rspec/rspec-rails/)
* 用于端到端集成测试的[Capybara](https://github.com/teamcapybara/capybara)

希望把上手这两个测试工具的过程做个记录：
* 一来了解了DSL的来历后可以熟练使用这些功能。
* 二来记录如何上手新的工具，以便激励自己继续前行。

#### 搭建开发环境（Mac OS）
* 先安装 [GDK](https://gitlab.com/gitlab-org/gitlab-development-kit/blob/master/doc/prepare.md#macos) 。
* 再用源码安装的方式安装一套 GitLab，以便优化公司的 GitLab。[参考](https://github.com/WebEntity/Installation-guide-for-GitLab-on-OS-X)
* 把 postgresql 摸了一遍，根据[参考资料](https://www.lijiaocn.com/%E6%8A%80%E5%B7%A7/2018/09/28/postgres-user-manage.html)搞定了git账号登陆的问题。
> 停止 postgresql 服务的方法：launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist  
> git 登陆 postgresql 的命令：psql -d gitlabhq_dev -Ugit -h 127.0.0.1  
> pg.conf 的变更： host    all             all             127.0.0.1/32            password  
* 原本想偷个懒，git 账号下ruby计划直接用sudo账号下的rvm，结果执行bundle失败，还是规规矩矩按照参考文档里面的安装rbenv。rbenv慢的问题可[参考](https://www.cnblogs.com/f-ck-need-u/p/10805521.html#installslow)

#### let 与 let!  

如果能正确地写出下面各项expect的结果，意味着搞懂了 let 和 let! 的区别。 

```ruby
$count1 = 0
$count2 = 0
RSpec.describe "let!" do
  invocation_order1 = []
  invocation_order2 = []

  let(:count1) do
    invocation_order1 << :let
    $count1 += 1
  end

  let!(:count2) do
   invocation_order2 << :let!
   $count2 += 1
  end

  it "calls count2 before hook and calls count1 in example" do
    invocation_order2 << :example
    expect(invocation_order2).to eq([:let!, :example])
    expect(invocation_order2).to eq([:let!, :example])
    expect(count2).to eq(1)
    expect(count2).to eq(1)

    invocation_order1 << :example
    expect(count1).to eq(1)
    expect(invocation_order1).to eq([:example, :let])
  end

  it "verifies the function of let!" do
    expect(invocation_order2).to eq([:let!, :example, :let!])
    expect(count2).to eq(2)
  end

  it "wants to know whether we can call the memoized helper method in an example" do
    invocation_order2 << :example
    count2()
    expect(invocation_order2).to eq([:let!, :example, :let!, :let!, :example])

    # We found that we can't call the memoized helper method in an example by ourselves"
    #expect(count).to eq(4)
    expect(count2).to eq(3)
  end
end
```


