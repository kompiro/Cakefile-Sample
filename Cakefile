{spawn} = require 'child_process'
fs = require 'fs'
path = require 'path'

MODEL_TEST_PATH = 'test/unit/models'
VIEW_TEST_PATH = 'test/unit/views'
TEST_PATHS = [MODEL_TEST_PATH,VIEW_TEST_PATH]

task 'test', (options)->
  {EventEmitter} = require 'events'
  emitter = new EventEmitter
  emitter.once 'expanded',(paths)->
    runMocha paths
  target = ""
  pathEmitter = new EventEmitter
  count = 0
  pathEmitter.on 'finished',(paths)->
    count += 1
    emitter.emit 'expanded',paths if TEST_PATHS.length is count
  TEST_PATHS.forEach (testPath)->
    expansionPath testPath,(result)->
      target += result
      pathEmitter.emit 'finished',target

task 'test_models', (options)->
  expansionPath MODEL_TEST_PATH,(target)->
    runMocha target

task 'test_views', (options)->
  expansionPath VIEW_TEST_PATH,(target)->
    runMocha target

runMocha = (target)->
  spawn  "sh",['-c',"`npm bin`/mocha -G #{target} -R spec"],customFds : [0, 1, 2]

expansionPath = (target,callback)->
  expansion = ""
  fs.readdir target,(err,files)->
    files = files.map (file)->
      return path.join(target,file)
    files.forEach (file)->
      expansion += file + ' '
    callback expansion

