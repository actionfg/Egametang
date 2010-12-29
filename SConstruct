env = DefaultEnvironment()

def ParseOptions():
	AddOption('--mode',
		dest='mode',
		action='store',
		type='string',
		default='dbg',
		help='build in opt mode, default dbg mode')
	AddOption("--ntest",
	    action="store_true",
	    dest="ntest",
	    default=False,
	    help="dont build test")

ParseOptions()

env.Append(LIBS=[
	'gflags',
	'glog',
])

if GetOption('mode') == 'opt':
	env['MODE'] = 'Opt'
	env.Append(CCFLAGS='-O2 -g')
	env.Append(LIBS='tcmalloc')
else:
	env['MODE'] = 'Dbg'
	env.Append(CCFLAGS='-g')
	env.Append(LIBS='tcmalloc_debug')

env['NTEST'] = GetOption('ntest')

env.Append(CPPPATH=Dir(env['MODE']).abspath)

def Test(env, target, source):
	if env['NTEST']:
		return
	test_env = env.Clone()
	test_env.Append(LIBS=[
		'gtest',
		'gmock',
	])
	test_target = test_env.Program(target, source)
	return test_target

env.AddMethod(Test, 'Test')

Export('env')

SConscript('Src/SConscript', variant_dir=env['MODE'], duplicate=0)

