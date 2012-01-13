{spawn} = require 'child_process'

task 'test', (options)->
  spawn __dirname + '/node_modules/mocha/bin/mocha',[],customFds : [0, 1, 2]
