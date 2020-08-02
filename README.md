# Babel-plugin-logger-source

Prepends file name and line numbers for each logger command, based on the source files.

```javascript
// src/view/app.js
class App() {
    constructor() {
        logger.log('test')//line no 20,column no 10
    }
}
↓ ↓ ↓ ↓ ↓ ↓
class App() {
    constructor() {
        logger.log('[s.V/app.js (20:10)]', 'test')
    }
}

```

### Usage

```bash
$ yarn add babel-plugin-logger-source -D
```

.babelrc

```javascript
{
    "plugins": [
        ["logger-source"]
    ]
}
```

### Options

.babelrc

```javascript
{
    "plugins": [
        ["logger-source",{
			    logger: string[];
				prefix: string;
				resolveFileName: Function | 'acronyms' | 'fullpath';

		}]
    ]
}
```

#### logger

Prepends for these command.
defaults:

```
logger:[
	'logger.log',
	'logger.info',
	'logger.wran',
	'logger.error',
	'logger.debug',
]
```

#### prefix

prefix before filename.  
example: `prefix:crm-client`。

```
logger.info('test');
↓ ↓ ↓ ↓ ↓ ↓
logger.info('crm-client [s.V/app.js (20:10)]', 'test')

```

#### resolveFileName

revolve the filename. support:`fullpath`,`acronyms` or custome function.default `acronyms`。

**acronyms**

```
// src\\View/app.js

logger.info('test');
↓ ↓ ↓ ↓ ↓ ↓
logger.info('[s.V/app.js (20:10)]', 'test')
```

**fullpath**

```
// src\\View/app.js

logger.info('test');
↓ ↓ ↓ ↓ ↓ ↓
logger.info('[s.V/app.js (20:10)]', 'test')
```

```
// src\\View/app.js

logger.info('test');
↓ ↓ ↓ ↓ ↓ ↓
logger.info('[src\\View\\app.js (2:10)]', 'test')
```

**Custom Fuction**  
`((params: ResolveFileNameParams) => string)`

params:

```js
params: {
  filename: string; //file name
  projectPath: string; //project path
  prefix: string; //the prefix config in babelrc
  line: number; //line no
  column: number; //column no
}
```

example:

```
{
    "plugins": [
        ["logger-source",{
				resolveFileName:function(params){
					return params.filename;
				}

		}]
    ]
}

```
