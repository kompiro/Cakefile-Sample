{spawn} = require 'child_process'
fs = require 'fs'
path = require 'path'

MODEL_TEST_PATH = 'test/unit/models'
VIEW_TEST_PATH = 'test/unit/views'
TEST_PATHS = [MODEL_TEST_PATH,VIEW_TEST_PATH]

task 'test', (options)->
  target = ""
  TEST_PATHS.forEach (testPath)->
    target += expansionPath testPath
  runMocha target

task 'test_models', (options)->
  target = expansionPath MODEL_TEST_PATH
  runMocha target

task 'test_views', (options)->
  target = expansionPath VIEW_TEST_PATH
  runMocha target

runMocha = (target)->
  spawn  "sh",['-c',"`npm bin`/mocha -G #{target}"],customFds : [0, 1, 2]

expansionPath = (target)->
  files = fs.readdirSync target
  files = files.map (file)->
    return path.join(target,file)
  expansion = ""
  files.forEach (file)->
    expansion += file + ' '
  return expansion

