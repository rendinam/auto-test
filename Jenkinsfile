println("env.BRANCH_NAME = ${env.BRANCH_NAME}")

@Library('utils@refactor') _

// [skip ci] and [ci skip] have no effect here.
if (utils.scm_checkout(['skip_disable':true])) return

// Allow modification of the job configuration, affects all relevant build configs.
// Pass this object in the argument list to the`run()` function below to apply these settings to the job's execution.
jobconfig = new JobConfig()
jobconfig.post_test_summary = true


// Pytest wrapper
def PYTEST_BASETEMP = "test_outputs"
def PYTEST = "pytest \
              -r s \
              --basetemp=${PYTEST_BASETEMP} \
              --junit-xml=results.xml"

// Configure artifactory ingest
data_config = new DataConfig()
data_config.server_id = 'bytesalad'
data_config.root = '${PYTEST_BASETEMP}'
data_config.match_prefix = '(.*)_result' // .json is appended automatically


bc0 = new BuildConfig()
bc0.nodetype = 'RHEL-6'
bc0.name = 'First'
bc0.env_vars = ['VAR_ONE=1',
               'VAR_TWO=2']
bc0.conda_packages = ['python=3.6',
                     'pytest=3.8.2']
bc0.build_cmds = ["ls -al",
                 "date"]
bc0.test_cmds = ["printenv | sort",
                "${PYTEST}"]
bc0.test_configs = [data_config]



bc1 = utils.copy(bc0)
bc1.name = 'Second'
bc1.env_vars = ['VAR_THREE=3',
               'VAR_FOUR=4']



utils.run([bc0, bc1, jobconfig])
