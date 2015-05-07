# gulp-seajs-package

***
> seajs(CMD) Module combo pulgin for gulp

## Install

```
$ npm install --save-dev gulp-seajs-package
```

## Usage

```
var gulp = require( 'gulp' ),
    seajsPackage = require( 'gulp-seajs-package' );
    
gulp.task( 'seajspackage', function(){
    return gulp.src( 'src/**/*.js' )
        .pipe( seajsPackage({
        	basePath:__dirname+"/src",
    	}))
        .pipe( gulp.desk('./build') );
}); 
```

## API

### seajsPackage( options )

Unsupported files are ignored.

### options

#### basePath
Type : `String`

same as seajs.config.base

#### encoding 

Type : `String`

Default : `utf-8`

#### ignore

Type : `Array`

Ignored module list. combo module `main`, need ignore dependencies `global` and `common`, configuration ignore list : 

```
ignore : [ 'global', 'common' ]
```

Ignore configuration has two rules, if need ignore `src/a` and `src/test/a`:

```
ignore : [ 'a' ]
```

If `src/a` and `src/test/a` need ignore `src/test/a`

```
ignore : [ 'src/test/a' ]
```



## Combo rule

Module `a.js` :

```
define(function(){
    var b = require( 'deps/b' );
    return 'a' + ' ' + b;
});
```

Module `b.js` :

```
define(function(){
    return 'b';
});
```

gulp code :

```
gulp.src( 'src/a.js' )
    .pipe( seajsPackage() )
    ...
```

Combined `a.js` :

```
define('b',function(){
    return 'b';
});
define('a',['b'],function(){
    var b = require( 'b' );
    return 'a' + ' ' + b;
});
```

File `main.js` :

```
seajs.use( 'a' );
```

Gulp code : 

```
gulp.src( 'src/main.js' )
    .pipe( seajsPackage() )
    ...
```

Combined `main.js` :

```
define('b',function(){
    return 'b';
});
define('a',['b'],function(){
    var b = require( 'b' );
    return 'a' + ' ' + b;
});
seajs.use( 'a' );
```



## License

MIT 