🐾 LocaleProvider

🕘 2019.11.06 由 hoanfirst 编辑


../../utils

```javascript

const defaultLang = 'zh';

const Util = {
    getLocal: function() {
        let lang = Util.getCookie('locale');
        return lang || defaultLang;
    },
    getCookie: (name) => {
      // locale存储在本地。
    },
}

export default Util;

```

```javascript

/**
 * @auther hoanfirst
 * 多语言配置
 */
 
import Util from '../../utils';

import zh_CN from '../../locale/zh_CN';
let customLocaleData = {
    "zh": zh_CN,
};

let locale = Util.getLocal();

Locale = {
  getMessage = (key, defaultMessage) => {
      let msg = customLocaleData[locale]["my_locale"][key];
      if (msg == null) {
          if (defaultMessage != null) {
              return defaultMessage;
          }
          return key;
      }
      return msg;
  };
}

export default LocaleProvide;

```
