{spawn} = require 'child_process'
fs = require 'fs'
path = require 'path'
{log,error} = require 'util'

MODEL_TEST_PATH = 'test/unit/models'
VIEW_TEST_PATH = 'test/unit/views'
TEST_PATHS = [
  MODEL_TEST_PATH,
  VIEW_TEST_PATH
]

TEST_COMMAND="#{__dirname}/node_modules/.bin/mocha"
TEST_OPTIONS='--growl --report spec'

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
  log 'start test'
  args = "#{TEST_OPTIONS} #{target}".trimRight().split(' ')
  spawn  "#{TEST_COMMAND}",args,customFds : [0, 1, 2]

expansionPath = (target,callback)->
  expansion = ""
  fs.readdir target,(err,files)->
    if !files
      error "Files aren't found. path:'#{target}'"
      return
    files = files.map (file)->
      return path.join(target,file)
    files.forEach (file)->
      expansion += file + ' '
    callback expansion

