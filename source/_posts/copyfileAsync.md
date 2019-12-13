---
title: node 异步递归拷贝文件
tags:
  -- node
categories: 
  - node  
---

```
var fs = require('fs'),
    path = require('path');
const config = require('../config');
function cpf(source, dest,  callback) {
    var cback = callback;

    var basePath = config.rootDir,
        currentPath = path.resolve(basePath, source),
        targetPath = path.resolve(basePath, dest),
        errs = null,
        started = 0,
        finished = 0,
        running = 0;

  
    startCopy(currentPath);

    function startCopy(source) {
        started++;
        var stat = fs.stat;
        running++;
        stat(source, function (err, stats) {
            var item = {};
            if (err) {
                return onError(err);
            }

            // We need to get the mode from the stats object and preserve it.
            item.name = source;
            item.mode = stats.mode;
            item.mtime = stats.mtime; //modified time
            item.atime = stats.atime; //access time

            if (stats.isDirectory()) {
                return onDir(item);
            }
            else if (stats.isFile()) {
                return onFile(item);
            }
        });
    }

    function onFile(file) {
        var target = file.name.replace(currentPath, targetPath);
        isWritable(target, function (writable) {
            if (writable) {
                return copyFile(file, target);
            }
            cb();
        });
    }

    function copyFile(file, target) {
        var readStream = fs.createReadStream(file.name),
            writeStream = fs.createWriteStream(target, { mode: file.mode });

        readStream.on('error', onError);
        writeStream.on('error', onError);

        writeStream.on('open', function () {
            readStream.pipe(writeStream);
        });

        writeStream.once('finish', function () {
            cb();
        });
    }

    function onDir(dir) {
        var target = dir.name.replace(currentPath, targetPath);
        isWritable(target, function (writable) {
            if (writable) {
                return mkDir(dir, target);
            }
            copyDir(dir.name);
        });
    }

    function mkDir(dir, target) {
        fs.mkdir(target, dir.mode, function (err) {
            if (err) {
                return onError(err);
            }
            copyDir(dir.name);
        });
    }

    function copyDir(dir) {
        fs.readdir(dir, function (err, items) {
            if (err) {
                return onError(err);
            }
            items.forEach(function (item) {
                startCopy(path.join(dir, item));
            });
            return cb();
        });
    }
 

    function isWritable(path, done) {
        fs.lstat(path, function (err) {
            if (err) {
                if (err.code === 'ENOENT') return done(true);
                return done(false);
            }
            return done(false);
        });
    }

    function onError(err) {
        if (!errs) {
            errs = [];
        }
        if (typeof errs.write === 'undefined') {
            errs.push(err);
        }
        else {
            errs.write(err.stack + '\n\n');
        }
        cb();
    }

    function cb(skipped) {
        if (!skipped) running--;
        finished++;
        if ((started === finished) && (running === 0)) {
            if (cback !== undefined) {
                return errs ? cback(errs) : cback(null);
            }
        }
    }
}

module.exports = cpf;
```