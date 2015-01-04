##Git fork

####更新自己Fork的代码项目和原来作者的项目进度一致的方法

当你fork并clone一个项目后，过一段时间，有可能作者原来的代码变化很大，你想接着在他最新的代码上修改，这时候你需要合并原作者最新代码过来，让你的项目变成最新的。

例如我fork了sri的Mojo项目，我的项目地址是https://github.com/iakuf/mojo 我现在克隆到本地了。

	git clone https://github.com/iakuf/mojo
	cd mojo

接着，我们需要添加sri项目的地址，也就是主项目的remote地址，我们加入后，给代码fetch过来，然后进行merge合并操作。

	git remote add sri https://github.com/kraih/mojo
	git fetch sri
	git merge sri/master
	
这样就能给你的当前本地的项目变成和原作者主项目一样。然后你按正常的流程进行修改并提交到你的项目就好了。

	git commit -am "更新到原作者的主分支进度"
	git push origin