解决kaggle日志不会自动滚动问题     
打开日志页面，按下f12，打开控制台，粘贴以下内容
```
(function() {
    console.log("Kaggle 日志自动滚动脚本已启动...");
    let lastScrollHeight = 0;
    
    // 每 1000 毫秒检查一次是否有新日志
    const scrollInterval = setInterval(() => {
        // 自动定位 Kaggle 的日志容器（通常是包含 log 或 terminal 关键字的 div）
        // 如果自动定位失败，脚本会尝试寻找页面中唯一一个高度在增加的可滚动元素
        const logContainers = Array.from(document.querySelectorAll('div'))
            .filter(el => {
                const style = window.getComputedStyle(el);
                return style.overflowY === 'auto' || style.overflowY === 'scroll';
            });

        logContainers.forEach(container => {
            // 如果容器的内容高度增加了，说明有新日志
            if (container.scrollHeight > container.clientHeight && container.scrollHeight !== lastScrollHeight) {
                // 滚动到底部 (你可以修改这里的逻辑来实现“偏下”的位置)
                // 默认滚到底部：
                container.scrollTo({
                    top: container.scrollHeight,
                    behavior: 'smooth'
                });
                
                // 如果你想要“屏幕中间偏下”的效果（例如预留 100px 缓冲）：
                // container.scrollTop = container.scrollHeight - container.clientHeight - 100;
                
                lastScrollHeight = container.scrollHeight;
            }
        });
    }, 1000);

    console.log("提示：如果想停止滚动，请输入: clearInterval(" + scrollInterval + ")");
})();
```
