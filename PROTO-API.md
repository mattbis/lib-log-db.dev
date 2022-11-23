# lib13891u3891u398u3

- now to actually implement this )) erm what langauge.. im thinking go for now .. since the other projects are already really hard to think about.. but im kinda writing it like Python... interesting.. maybe I could use cosmo and python or go and python and this will be a part of msmeo..  

- or well rust is kinda growing slowly on me at first it looked weird ..

- todo tomorrow instead last is parity yes .. but a parity diff to the last store which is better.. + we can have a ghost tail gate which is part of verify... 

- its time to consolidate now the api has stuff a wrapper does and the core does in one. its best to split it.

### todo

- sep of static, thing, dyn, mixin, subclass, mixclass, cli, library, core, noargs, 32bit, win, lin, mac

- mode should control the sys usage in a clear way aside from the main controls ( this is for a very simple machine )
- most of this is in the wrapper - the dyn parts
- shared and private 
- rkey wkeys ckeys 
- mrkeys mwkeys erkeys ewkeys
- --safe --paranoid --casual --forgetful < someting like this for how it works for safety,, im prprobably not doing safe ,,, just paranoid.. i dont need it 
im mostly thinkign aobut tools on a safe machine where i dont cacre about a lot of stuff

# all
- the order of calls determines what happens... it just does the min expected. to script it means using op_seq()
- since im using htis one way for the moment is like this.. ( in some places auto behaviour ) 

## cli
### exit codes
- todo

### constructor
`ltl`
- set dynamically or invoke n in the struct - this shoudl be renamed for a simplistic structure which is the indendedated usage...
`ltl._CACHE`
`ltl._TOPIC`
`ltl._FILTERS`

### config / note you can use args or shortcuts... 
- `ltl.config()`
- `ltl.config('logger.path')('/home/myspecialuser/evt.log')` - defaults to `/var/somewhere i forgot/ltl/logs/evt.log`

#### cli, config.keys, .env or defaults `/var/somewhere i forget` || `AppData/local/lib-` etc (give name)
or config
1. `ltl [cmd] [options]`
2. `ltl --cmd [options]`
3. `ltl --config [keys=val,..N] cmd|--cmd [options]`

### this is two things atm moment so it takes a bit of reading .. 
##### finite control -- todo split up the easy shortcuts 
the below is confusing we have classes of config.. split the wrapper and lib
- `force-all.path` - contain it in one place ... `/opt/ltl` - if not its Null

evt is the base logger of the server, process.. whatever you do it will write.. unless it cant.
- `limit.nodes` 0
- `root.spread`
- `root.logger.evt.path` - set before running if want custom.. but this log is small....
- `root.heap.path`
- `root.dump.path`
- `root.crash_handler.path` - this seems a good idea if you did if yourself .. 
- `args.verbose` | defaults.verbose - cli messages
- `args.debug` | defaults.debug - cli messages
- `op.queue.disk.path` | defaults var root... 
- `logger.path`
- `logger.flags.path`
- `logger.signals.path`
- `logger.op.path`
- `logger.server.path`
- `should.integrity` - set this to reduce the frequency of integrity operations ( like a friendly command line switch ie.. --paranoid --okay ) etc
- `should.os-test` - stop it doing this if a retry happens - instead forcing a fail
- `integrity.op.path` - this will dump the operation before it tries it etc - but default None
- `integrity.dump.before.path` - setting this enables it ... probably - dont enable this 
- `integrity.dump.last.path` - dont enable this
- `manifests.path`
- `scratch.path` - this is due to the manifest envelope, and whatever is being written .. although it will chunk it. its teh queue path...
- `force.readkeys`
- `force.writekeys`
- `force.chkeys`
- `force.no-net`
- `force.one.core`
- `force.one.vol`
- `can.index`
- `can.reserve` - effectively controls caching and indexes... can be mem val
- `can.reserve.mem`
- `can.reserve.disk`
- `can.setup`
- `can.install`
- `can.serve`
- `can.listen`
- `can.shared-mem`
- `can.volume`
- `can.gpu`
- `can.args` - no args work whatsoever
- `can.stat` - if its stable you can stop it bothering... 
- `can.os.dirs` - dont use any os stuff just use memmmmmmoooorry
- `can.os.test` - dont use os test methods for operations...
- `can.user.mac-uu`
- `must.strict` - will sanify and stop if anything goes wrong.. 0 means forgive a bit
- `must.fatal` - you can forgive, but if a fatal happens exit... will stop earlier than forgive
- `debug.verify` - it will check every operation again...  in both places..
- `must.user` - use hash and user and os only + eth mac addresss
- `must.sanify` - the message is checked for weird stuff
- `must.sparse` - dont reuse any nodes.. 
- `must.protected` - dont use shared memory ( its not really aimed at these things but singular usages on safe machines.. ) - sets can.shared-mem
- `can.disk` - you cant have both 0 <== only use for testing - since very dangerous
- `can.ram` - this means it wont use any buffer but minimum and will be very very slow....  todo later on you can give a finite for whatever system to run on .
- `must.os`
- `must.wait.[op_code]` - this is due to some systems not being usual or disks or whatever, etc.... 
- `force.retry.[op_code]` - tailor

### runtime/config/message envelope
- `ltl.envelope_hash(algor)`
- `ltl.message_hash(algor)`
- `ltl.manifest_index_hash(algor)`
- `ltl.NONE`
- `ltl.SEMVER_VERSION`
- `ltl.VERSION` derived semver serial + buildnumber
- `ltl.DEFAULT_DATE_SYSTEM` - enum.... default is iso
- `ltl.DEFAULT_DATE_PRECISION` - enum... defaults are ms
- `ltl.DEFAULT_APPEND_OPTIONS`
  - { ?append_interval: 2[0|pms|pn|pt|po|ms] }

- `ltl.BUILD_MANIFEST`
  - { version, semver_version, number, builder, machine, date, priv_key_flag, repo_key_flag, options, flags, ltl_build_flags, ltl_build_headers_hash, build_hash, build_os, build_n, build_s }
-- key needed for dyn usage of lib
  - `ltl.DEFAULT_RKEY[]`
  - `ltl.DEFAULT_WKEY[]`
  - `ltl.DEFAULT_CKEY[]`
- `ltl.DEFAULT_NODE`
- `ltl.DEFAULT_MEM`
- `ltl.DEFAULT_LINK`
- `ltl.DEFAULT_SPARSE_ENVELOPE`
- `ltl.DEFAULT_SPARSE_MANIFEST`
- `ltl.DEFAULT_SPARSE_MESSAGE`
- `ltl.DEFAULT_ENVELOPE`
  --> `{v:'', sys_v:'',dw:'',user:'',machine:'',?pid,?path,?process,?std_in_last,?std_in_range,?std_error}`
- `ltl.DEFAULT_MANIFEST`
  --> `{ ?...DEFAULT_ENVELOPE, ?...BUILD_MANIFEST, ?custom_env }` - this is like a construct you can pass a number to control what it does, or give it an object based on the defaults... or make one that it will cache... ( manifest will do whatever you tell it - get or set if it doesnt exist, if it clashes another is created )
- `ltl.DEFAULT_MESSAGE[N=/dev/random{6}]`
  --> `{ 'ltl-foo%N', %evt_log%%last.count }
- `ltl.manifests()` it always uses the current but keeps them since its handy to see what somebody or I did before I forget or make a mistake
- `ltl.manifest()` ==> `manifest_index : BigInteger` get or set a new one
- `ltl.sparse_manifest()` ==> limited version ( it has to be signed / this can automatically just take a bit of stuff,, and limit )

- `ltl.message()` this controls how messages registered like above ,, its the same as envelope in many places

### data types
##### index
- defaults: bigInts, hashes ( its not the silly )

#### message
- can be anything... 

### private api -todo

- `ltl.sanify()` its possible weird chars
- `ltl.integrity_check()` this blocks and might be called - always on startup
- `ltl.hook(symAppend,({data}) => ltl.OP[copy](last, data)`

- `ltl.CACHE`
- `ltl.cache()` keys, struct, used by a lot of things...
  - `ltl.cache_frags(_,_,pr)`
  - `ltl.cache_export(ltl.CACHE.EXPORT_TYPE= default | bit)`
  - see _CACHE you cannot restore a cache completely just parts of it based on runtime... however it will significantly improve all speeds... precache... statically.

- `ltl.OP.__mandatory()` checks things exists, checks hashes match
- `ltl.safe_lookup()`

- `ltl.reservation(symDISK)`

#### index ==>

sometihngi is botherign me here having message and envelope read keys in private memory.... im missing it but its part of the below i think... 

- `ltl.definition(topic)` matches a filter struct
 
- `ltl.defaults(ltl._TOPIC,_)` index via default into topic cache
- `ltl.reserve(ltl._CACHE)` todo this is gonna be not this simple since it also is another parto f this i ahvent added yet 

- `ltl.index(ltl.DEFAULT_FILTERS)`
- `ltl.index({prop_match,val_match,int_match})`

- `ltl.lookup()` - util method for below
- `ltl.suggest()` 
- `ltl.query()`
- `ltl.filter()` - create or get ( pre cache some struct )

- `ltl.one(symMANIFEST|symDATA, filter)` --> also ltl.verify()
- `ltl.range(..)`
- `ltl.some(..)`

- `ltl.flush(..)`

## stores
- `ltl.store_keys()`
- `ltl.vol_keys()`
- `ltl.seek(..)`
- `ltl.raw(..)`
- `ltl.spread(..)`
- see: ltl.volumes()

## nodes ==> todo(wip) v2
.. + parity for tmrw
##### ref, link ref() link()

### protected api - debugging - dev
- `ltl.dangerously_flush_queue()`
- `ltl.dangerously_reset_instance()`
- `ltl.dangerously_allow_env()`
- `ltl.dangerously_disable_sanify()`
- `ltl.dangerously_disable_disk_state()`
- `ltl.dangerously_unseal_config()` - sealed by default
- `ltl.dangerously_emit_signal()`
- `ltl.dangerously_manifest_replace(index : BigInteger)` oops
- `ltl.dangerously_disable_store_keys()`

### public api

#### symbols - use in wrapppper - obviously the below is not really how its gonna work just a starting point since they would be too hard.. 
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
- `ltl.symSTRUCT` filter / index / other stuff when i think about it 
- `ltl.symVOL` virtual / real / volatile / sticky 
- `ltl.symRAW` used for unmanaged files level stack
- `ltl.symDisk` the actual device

- `ltl.symNONE` - you can use this to just write a test envelope/message or wherever its the same as `_`
- `ltl._` - this means partial for some thing... if you need it

#### op codes are the method names just like `ltl.hook(op_code|method_id|error_code)` probably...

- `ltl.OP(code|method)` call it
- `ltl.OP.default()` get the default
- `ltl.dimpl.OP[{0,init}]` default impl linked to various

#### error codes
- `ltl.ERROR(code|name)`
- `ltl.dimpl.ERROR[0,init]` --> usually is going to point to `ltl.dimpl.ERROR`

#### methods

- `ltl.init()`
- `ltl.setup()`
- `ltl.mandatory()` 

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
- `ltl.flags.linked`
- `ltl.flags.cleanup`
- `ltl.flags.hashing`
- `ltl.flags.file_lock`
- `ltl.flags.key_lock`
- `ltl.flags.store`
- `ltl.flags.gpu`

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
  - `ltl.db.log`
  - `ltl.db.mem`
  - `ltl.db.struct`
  - `ltl.emitter`
  - `ltl.Envelope`
  - `ltl.Flag` make it do something via .impl.
  - `ltl.Message`
  - `ltl.Manifest`
  - `ltl.Server`
  - `ltl.ERROR`
  - `ltl.FLAGS` just have one
  - `ltl.OP` -> implements OP.__mandatory
  - `ltl.warn`
  - `ltl.run`
  - `ltl.DEFAULTS`
  - `ltl.user`
  - `ltl.index`
  - `ltl.store`
  - `ltl.os.integrity()` -> checks we have dispace etc and whatever else... you can call it it will queue itself
  - `ltl.os.volume()`
  - `ltl.os.net()`
  - `ltl.os.net.uu-mac-id()`
  - `ltl.os.gpu()`
  - `ltl.aimpl` 
  - use abstract instead and do weird stuff -- its likely a bit will be here if i dont have time..
  - `--abstract` - provide piped script abstracts
- `ltl.impl` should be coded by somebody else better than me .. 
  - `ltl.op.__mandatory` - stops borking of data - see behaviours
  - `ltl.db.log.matt` - simple db .get() .set()
  - `ltl.db.struct.matt` - simple default struct
  - `ltl.db.mem` - default implm
  - directly insert whatever - this is much faster than user .hook() or events.. 
### Impl
- `ltl.Impl`
  - `ltl.Impl.OP_CODES`
  - `ltl.Impl.ERROR_CODES`
  - `ltl.Impl.WARN_CODES`
  - `ltl.Impl.BEV_CODES`
  - `ltl.Impl.Behaviours`
  - `ltl.Impl.Emitter`
  - `ltl.Impl.FlagRecord()`
  - `ltl.Impl.Listener`
  - `ltl.Impl.Server`
  - `ltl.Impl.OP.__mandatory` -> ensure integrity
  - `ltl.Impl.Runner` build, cli etc
  - `ltl.Impl.Defaults`
  - `ltl.Impl.User` -> this is the default way to identify you
  - `ltl.Impl.DB`
  - `ltl.Impl.Index`
  - `ltl.Impl.OS`
  - `ltl.Impl.OS.Volume`
  - `ltl.Impl.OS.Gpu`
  - `ltl.Impl.net`
  - `ltl.Impl.HQ` evt queue
  - `ltl.Impl.PQ` op queue
  - `ltl.Impl.RQ` runner queue
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
- `ltl.behaviours()`
- `ltl.read()`
- `ltl.flags()`
- `ltl.run()` turns args into ops, translate
- `ltl.net()` uu-mac-id
- `ltl.interfaces()`
- `ltl.running()`
- `ltl.op(CODE)` call one operation 
- `ltl.ops()` ==> codes()
- `ltl.op_seq()`
- `ltl.op_codes()`
- `ltl.op_restore()`
- `ltl.op_export()`
- `ltl.gpu()`
- `ltl.cleanup()`
- `ltl.errors()`
- `ltl.error_codes()`
- `ltl.volumes()`

##### these are micro tasks ... 
- `ltl.OS_VALIDATOR.testPath()`
- `ltl.OS_VALIDATOR.testVolume()`
- `ltl.OS_VALIDATOR.testExecution()`
- `ltl.OS_VALIDATOR.testSubProcess()`
- `ltl.OS_VALIDATOR.testPermissions()`
- `ltl.OS_VALIDATOR.testUserMatches()`
- `ltl.OS_VALIDATOR.checkUserChange()`
- `ltl.OS_VALIDATOR.testDataWrite()`
- `ltl.OS_VALIDATOR.testDataRead()`
- `ltl.OS_VALIDATOR.testDataChMod()`
- `ltl.OS_VALIDATOR.testTemp()`
- `ltl.OS_VALIDATOR.testProgramDataRead()`
- `ltl.OS_VALIDATOR.testProgramDataChMod()`
- `ltl.OS_VALIDATOR.testProgramDataWrite()`

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
- `ltl.Server()`

- unsure what language im using .. 

## graveyard - nusery - incubator
- `ltl.force_manifest_version_append()`
- `ltl.force_append_interval()`
- `ltl.clean()` not sure at the moment.. 

- `ltl.symNODE`
- `ltl.symLINK`
- `can.fork` - cluster
- `can.link` - link nodes into priv memory
- `can.cleanup` - cleanup nodes
- `can.ref` - keep nodes

- `ltl.retry()`

- ?`ltl.cores(Int)`
- ?`ltl.threads(Int)`
- ?`ltl.gpu(Int)

ideas on resolution:-
- `config.mandatory` mem,disk,query,get,set,op,vp,sp,user,sys,mem,struct,cache,os,perf-agg,perf-g,perf-log,sys-log,dist-log,win-log,defrag

- `ltl.symPARITY`

- `ltl.Tree`
  - `ltl.Table`
  - `ltl.Graph`
  - `ltl.symTREE`

-- this is actually how i sohuld be thinkgin aobut last & dump except its a db sense but the terms shoudl be unified

` can.branch`

-- parity then becomes one of 3 main ENUMS + this is a bitfield you can set.. - to replace the above concerns etc

- FILE_PAR
- SOFT_PAR
- HARD_PAR
- 

- `ltl.retry()`

- symRaw, symDisk, 

- dangerous env, msg keys
- -- private key cache ( probably dangerous ) 
mkey,mymkey,myenvkey,defenvkey



- dedupe()
