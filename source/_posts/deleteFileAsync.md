---
title: node 异步递归删除文件
tags:
  -- node
categories: 
  - node  
---


```
var fs = require('fs');
const cpath = require('path');
const config = require('../config');
function removeRecursive (path,cb){
    var self = this;
    path = cpath.resolve(config.rootDir, path),

    fs.stat(path, function(err, stats) {
      if(err){
        cb(err,stats);
        return;
      }
      if(stats.isFile()){
        fs.unlink(path, function(err) {
          if(err) {
            cb(err,null);
          }else{
            cb(null,true);
          }
          return;
        });
      } else if(stats.isDirectory()){
        fs.readdir(path, function(err, files) {
          if(err){
            cb(err,null);
            return;
          }
          var f_length = files.length;
          var f_delete_index = 0;

          var checkStatus = function(){
            if(f_length===f_delete_index){
              fs.rmdir(path, function(err) {
                if(err){
                  cb(err,null);
                }else{ 
                  cb(null,true);
                }
              });
              return true;
            }
            return false;
          };
          if(!checkStatus()){
            for(var i=0;i<f_length;i++){
              (function(){
                var filePath = path + '/' + files[i];
                removeRecursive(filePath, (err,status) => {
                  if(!err){
                    f_delete_index ++;
                    checkStatus();
                  }else{
                    cb(err,null);
                    return;
                  }
                });
    
              })()
            }
          }
        });
      }
    });
  };
  const rmf = removeRecursive;
  module.exports = rmf;
```