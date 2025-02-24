# Nexus-3
Nexus网页自动连接

--------------------------

/**
 * 使用 XPath 获取元素节点。
 * @param {string} xpath - 用来定位元素的 XPath。
 * @return {Node|null} 返回找到的元素节点，如果未找到则返回 null。
 */
function getElementByXPath(xpath) {
    const result = document.evaluate(xpath, document, null, XPathResult.FIRST_ORDERED_NODE_TYPE, null);
    return result.singleNodeValue;
}

/**
 * 检查页面状态并决定是否点击按钮。
 */
function connectToNexus() {
    // 获取状态文本元素
    const statusElement = getElementByXPath("//p[contains(text(), 'CONNECT TO NEXUS') or contains(text(), 'CONNECTED')]");

    if (statusElement) {
        const statusText = statusElement.textContent.trim();
        console.log(`当前状态: ${statusText}`);

        // 只有在状态是 "CONNECT TO NEXUS" 时才点击按钮
        if (statusText === "CONNECT TO NEXUS") {
            console.log("准备点击按钮进行连接...");
            clickButton();
        } else if (statusText === "CONNECTED") {
            console.log("已经连接，无需点击。");
        } else {
            console.log("状态未知，不执行操作。");
        }
    } else {
        console.log("未找到状态文本元素，可能尚未加载。");
    }
}

/**
 * 查找并点击连接按钮。
 */
function clickButton() {
    const button = getElementByXPath("//div[contains(@class, 'cursor-pointer') and contains(@class, 'border-4')]");

    if (button) {
        button.click();
        console.log("按钮已点击！");
    } else {
        console.log("未找到按钮，可能尚未加载或 XPath 需要调整。");
    }
}

/**
 * 5秒后首次检查，然后每隔5秒检查一次状态。
 */
setTimeout(connectToNexus, 5000);
setInterval(connectToNexus, 5000);

-------------------------
