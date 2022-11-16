- now to actually implement this )) erm what langauge.. im thinking go for now .. since the other projects are already really hard to think about.. but im kinda writing it like Python... interesting.. maybe I could use cosmo and python or go and python and this will be a part of msmeo..  

- todo tomorrow instead last is parity yes .. but a parity diff to the last store which is better.. + we can have a ghost tail gate which is part of verify... 

# all
- the order of calls determines what happens... it just does the min expected. to script it means using op_seq()
- since im using htis one way for the moment is like this.. ( in some places auto behaviour ) 

## cli
### exit codes
- todo

### constructor
- todo lang

### config
- `ltl.config()`
- `ltl.config('logger.path')('/home/myspecialuser/evt.log')` - defaults to `/var/somewhere i forgot/ltl/logs/evt.log`

#### cli, config.keys, .env or defaults `/var/somewhere i forget` || `AppData/local/lib-` etc (give name)
or config
1. `ltl [cmd] [options]`
2. `ltl --cmd [options]`
3. `ltl --config [keys=val,..N] cmd|--cmd [options]`

- `force-all.path` - contain it in one place ... `/opt/ltl` - if not its Null
evt is the base logger of the server, process.. whatever you do it will write.. unless it cant.
- `root.logger.evt.path` - set before running if want custom.. but this log is small....
- `args.verbose` | defaults.verbose - cli messages
- `args.debug` | defaults.debug - cli messages
- `op.queue.disk.path` | defaults var root... 
- `logger.path`
- `logger.flags.path`
- `logger.signals.path`
- `logger.op.path`
- `logger.server.path`
- `should.integrity` - set this to reduce the frequency of integrity operations ( like a friendly command line switch ie.. --paranoid --okay ) etc
- `integrity.op.path` - this will dump the operation before it tries it etc - but default None
- `integrity.before.path` - setting this enables it ... probably
- `integrity.last.path`
- `manifests.path`
- `scratch.path` - this is due to the manifest envelope, and whatever is being written .. although it will chunk it. its teh queue path...
- `can.index`
- `can.reserve`
- `can.setup`
- `can.install`
- `can.args` - no args work whatsoever
- `can.stat` - if its stable you can stop it bothering... 
- `can.os.dirs` - dont use any os stuff just use memmmmmmoooorry
- `can.os.test` - dont use os test methods for operations...
- `must.strict` - will sanify and stop if anything goes wrong.. 0 means forgive a bit
- `must.fatal` - you can forgive, but if a fatal happens exit... 
- `must.verify` - it will check every operation again...  in both places...
- `must.user` - use hash and user and os only + eth mac addresss
- `must.sanify` - the message is checked for weird stuff
- `can.disk` - you cant have both 0 <== only use for testing - since very dangerous
- `can.ram` - this means it wont use any buffer but minimum and will be very very slow....  todo later on you can give a finite for whatever system to run on .
- `must.os`
- `must.wait.[op_code]` - this is due to some systems not being usual or disks or whatever, etc.... 

### runtime/config/message envelope
- `ltl.envelope_hash(algor)`
- `ltl.manifest_index_hash(algor)`
- `ltl.SEMVER_VERSION`
- `ltl.VERSION` derived semver serial + buildnumber
- `ltl.DEFAULT_DATE_SYSTEM` - enum....
- `ltl.DEFAULT_APPEND_OPTIONS`
  - { ?append_interval: 2[0|pms|pn|pt|po|ms] }
- `ltl.BUILD_MANIFEST`
  - { version, semver_version, builder, machine, date, priv_key_flag, repo_key_flag, options, flags, ltl_build_flags, ltl_build_headers_hash, build_hash, build_os, build_n, build_s }
- `ltl.DEFAULT_ENVELOPE`
  --> `{v:'', sys_v:'',dw:'',user:'',machine:'',?pid,?path,?process,?std_in_last,?std_in_range,?std_error}`
- `ltl.DEFAULT_MANIFEST`
  --> `{ ?...DEFAULT_ENVELOPE, ?...BUILD_MANIFEST, ?custom_env }` - this is like a construct you can pass a number to control what it does, or give it an object based on the defaults... or make one that it will cache... ( manifest will do whatever you tell it - get or set if it doesnt exist, if it clashes another is created )
- `ltl.DEFAULT_MESSAGE[N=/dev/random{3}]`
  --> `{ 'ltl-foo%N', %evt_log%%last.count }
- `ltl.manifests()` it always uses the current but keeps them since its handy to see what somebody or I did before I forget or make a mistake
- `ltl.manifest()` ==> `manifest_index : BigInteger` get or set a new one
- `ltl.sparse_manifest()` ==> limited version ( it has to be signed / this can automatically just take a bit of stuff,, and limit )

- `ltl.message()` this controls how messages registered like above ,, its the same as envelope in many places

### data types
##### index
- defaults: bigInts, hashes ( its not the silly )

### private api

- `ltl.sanify()` its possible weird chars
- `ltl.logger.set` log of operations
- `ltl.integrity_check()` this blocks and might be called - always on startup
- `ltl.hook(symAppend,({data}) => ltl.OP[copy](last, data)`

- `ltl.cache()`
  - `ltl.cache_frags(_,_,pr)`

#### index ==>
- `ltl.seek(ltl._CACHE)`
 
- `ltl.defaults(ltl._TOPIC)`
- `ltl.reserve(ltl._CACHE)`

- `ltl.index(ltl.DEFAULT_FILTERS)`
- `ltl.index({prop_match,val_match,int_match})`

- `ltl.lookup()` - util method for below
- `ltl.filter()` - create or get ( pre cache some struct )

- `ltl.one(symMANIFEST|symDATA, filter)` --> also ltl.verify()
- `ltl.range(..)`
- `ltl.some(..)`

- `ltl.raw(..)`

- `ltl.flush(..)

### protected api - debugging - dev
- `ltl.dangerously_flush_queue()`
- `ltl.dangerously_reset_instance()`
- `ltl.dangerously_allow_env()`
- `ltl.dangerously_disable_sanify()`
- `ltl.dangerously_disable_disk_state()`
- `ltl.dangerously_unseal_config()` - sealed by default
- `ltl.dangerously_emit_signal()`
- `ltl.dangerously_manifest_replace(index : BigInteger)` oops

### public api

#### symbols
- `ltl.symMANIFEST`
- `ltl.symDATA`
- `ltl.symREADS`
- `ltl.symWRITES`
- `ltl.symFATAL`
- `ltl.symAPPEND`
- `ltl.symBLOCKING`
- `ltl.symSLEEP_DEEP`
- `ltl.symSLEEP_NAP`
- `ltl.symIDLE`
- `ltl.symHARD_LOCK`
- `ltl.symSOFT_LOCK`
- `ltl.symPARITY`

- `ltl.symNONE` - you can use this to just write a test envelope/message or wherever its the same as `_`
- `ltl._` - this means partial for some thing... if you need it

#### op codes are the method names just like `ltl.hook(op_code|method_id|error_code)` probably...

- `ltl.OP(code|method)`
- `ltl.dimpl.OP[{0,init}]`

#### error codes
- `ltl.ERROR(code|name)`
- `ltl.dimpl.ERROR[0,init]` --> usually is going to point to `ltl.dimpl.ERROR`

#### methods

- `ltl.init()`
- `ltl.setup()`

- `ltl.get_symbol_keys()`

- `ltl.lock` `[1,ltl.symHARD_LOCK||ltl.symSOFT_LOCK]`
- `ltl.release` `[1]`
- `ltl.locked()`

- `ltl.mode` `ltl.mode_default` 0 & `ltl.mode_enable_custom` 1, | `ltl.mode_block` 3, | `ltl.mode_strict` 4, | `ltl.mode_forgive` 5, | `ltl.mode_slowdown` 7, | `ltl.mode_speedup` 8, | `ltl.mode_normal` 9 ==> `ltl.mode_matt`

- `ltl.append(data,pr)`
- `ltl.slow_append()` ==> append(_,9)

- `ltl.wait(1)` ==> `ltl.idle()`
- `ltl.idled()`

- `ltl.pending()` ==> `[pending_op_codes]`

- `ltl.export()` supported formats...

- `ltl.reserve()` todo

- maybe useful - by default would have a wakeup time...
its bothering me ... maybe i want
`ltl.sleep(ltl.symSLEEP_DEEP)` it will stop completely but the process is still running and will give a status ( you have to handle the error codes )
`ltl.sleep(ltl.symSLEEP_NAP)` it will just queue to disk very efficiently --> so it wont index data, or change teh store triggering the integrity instead its a buffer that grows and grows... 

`ltl.sleep(ltl.symWRITES)`
`ltl.sleep(ltl.symREADS)`
`ltl.awake()`
  - this would mean having to worry about .append() queuing data forever... depending on the type of sleep...

- see also OP_CODES

### flags - these are not to be confused with signals they just have a boolean value

- `ltl.FLAG(''||key)` - maybe are symbols also but its a lot of them... so instead... an internal flag hash... based on build..

- * -> symFlagOpen etc... todo

- 0 => `ltl.flags.open`
- 1 => `ltl.flags.write`
- .. => `ltl.flags.ready`
- `ltl.flags.fatal`
- `ltl.flags.warn`
- `ltl.flags.error`
- `ltl.flags.wait`
- `ltl.flags.parity` - state is completely persisted
- `ltl.flags.sleeping`
- `ltl.flags.verify`
- `ltl.flags.integrity`
- `ltl.flags.last` - means it makes a another copy before doing anything somewhere else on your system ( ie, some big old drive )
- `ltl.flags.backup` - means the op queue is backup itself something which then means parity.. parity will change almost constantly and is just fun
- `ltl.flags.sanify`
- `ltl.flags.blocking`
- `ltl.flags.installed`
- `ltl.flags.setup`
- `ltl.flags.slowdown` - just check 0 for normal there are only 2
- `ltl.flags.new` - it never run before since it will make `/var/somewhere i forgot/ltl/evt.log`
- `ltl.flags.waiting`
- `ltl.flags.pending`

### OPS
- `0..10`
  1. `init`
  2. `setup`
- `0..20`
  - ..

### ERRORS
- `0..10`
  - 0. ..

### PATHS

### FILTERS
`ltl.DEFAULT_FILTERS`

#### logger paths
- the logger uses a resolution of /var/usr | appDataRoaming | programData | local home dir for evt... this is the Server & friends, and when it ran... ie. installed, setup, new etc
- the bigger runtime logs are /var/somewhere i forget/logs/[thing]/[date][type][session].log

- `ltl.p['logger.flags.path']` => 'flags' - its just the default dir name based on the default paths 

### signals - can be used on tight integeration todo ( this i have not looked at for ages so im just using a vague memory hmmm - and im not doing it )
1. version 1 can be basic until i know about this some years later... 
- for some reason i thouoght of this the semaphore
- `ltl.SIGNAL()`
- `ltl.SIGNALS.symAPPEND`
- `ltl.SIGNALS.symAPPEND%%[use the os signal erm i forget]`

### stat

- `ltl.stat('writes')` - `ltl.stat('queue')` - `ltl.stat(symFlagBackup)`
- `ltl.stat()`

### inner queue 
- todo a kinda hash that is based on the last N entries
- it will grow forever if you call it too much.. but never fail however this needs remediation os wise etc.. 

## impl
- `ltl.current(keys)` || `ltl.DEFAULT_MANIFEST`
- `ltl.PHASE`=`%%START||%%EXIT`

### dimpl
- `ltl.dimpl`
  - `ltl.pq`
  - `ltl.hq`
  - `ltl.rq`
  - `ltl.msg`
  - `ltl.OP.*`
  - `ltl.db`
  - `ltl.emitter`
  - `ltl.Envelope`
  - `ltl.Message`
  - `ltl.Manifest`
  - `ltl.Server`
  - `ltl.ERROR`
  - `ltl.FLAGS`
  - `ltl.flag`
  - `ltl.OP` -> implements OP.__mandatory
  - `ltl.warn`
  - `ltl.run`
  - `ltl.DEFAULTS`
  - `ltl.user`
  - `ltl.index`
  - `ltl.os.integrity()` -> checks we have dispace etc and whatever else... you can call it it will queue itself
- `ltl.aimpl` 
  - use abstract instead and do weird stuff -- its likely a bit will be here if i dont have time..
- `ltl.impl`
  - `ltl.db.matt` - simple db
  - directly insert whatever - this is much faster than user .hook() or events.. 
### Impl
- `ltl.Impl`
  - `ltl.Impl.OP_CODES`
  - `ltl.Impl.ERROR_CODES`
  - `ltl.Impl.WARN_CODES`
  - `ltl.Impl.Emitter`
  - `ltl.Impl.Listener`
  - `ltl.Impl.Server`
  - `ltl.Impl.OP.__mandatory` -> ensure integrity
  - `ltl.Impl.Runner` - build
  - `ltl.Impl.Defaults
  - `ltl.Impl.User` -> this is the default way to identify you
  - `ltl.Impl.DB`
  - `ltl.Impl.Index`
  - `ltl.Impl.OS`
  - `ltl.Impl.HQ`
  - `ltl.Impl.PQ`
  - `ltl.Impl.RQ`
### Static
- `ltl.msg`
  - `ltl.msg(key|code|hash|static).key()`
    - `ltl.msg('en').symBlocking` = defaults ==> "warning all operations are sequential"
- `ltl.api()`
- `ltl.codes()` gets every unique code from library ( errors and ops dont clash they are in regions ) 
- `ltl.defaults()`
- `ltl.types()`
- `ltl.help()`
- `ltl.status()`
- `ltl.caches()`
- `ltl.read()`
- `ltl.run()`
- `ltl.running()`
- `ltl.op(CODE)` call one operation 
- `ltl.ops()` ==> codes()
- `ltl.op_seq()`
- `ltl.op_codes()`
- `ltl.op_restore()`
- `ltl.op_export()`
- `ltl.cleanup()`
- `ltl.errors()`
- `ltl.error_codes()`
- ?`ltl.cores(Int)`
- ?`ltl.threads(Int)`
- `ltl.OS_VALIDATOR.testPath()`
- `ltl.OS_VALIDATOR.testExecution()`
- `ltl.OS_VALIDATOR.testSubProcess()`

#### dynamic library hooking - use above in code
- `ltl.hookable()`
- `ltl.hook(ltl.OP_CODES[code])`
- `ltl.hook(ltl.ERROR_CODES[code])`
- `ltl.hook(ltl.FLAG[key])`
- `ltl.hook(ltl.PHASE)`
- `ltl.hook(ltl.symFATAL)()`
- `ltl.hook(ltl.symAPPEND)(externalPushWrite(data,ltl.current()))`
- `ltl.hook(ltl.symAPPEND)(exportToSomeOtherFormat(data))`

#### interface that should work with most common event type things - otoh i might just keep it really simple and old school
- `ltl.listen()`
- `ltl.emittable()`
- `ltl.emit()`
- `ltl.broadcast()`
- `ltl.stream()`

- unsure what language im using .. 

## graveyard
- `ltl.force_manifest_version_append()`
- `ltl.force_append_interval()`
