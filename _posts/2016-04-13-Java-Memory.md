---
layout: post
title:  "Java Memory"
date:   2016-04-13 10:28:46 +0900
categories: Java
---    
    
    jagadesh4java blogspot

Java Heap

    consists of...

    1) Young generation

      divided to 3 partitions
        |
        +-->1) Eden
        |
        |
        +-->2) S1 space
        |
        |
        +-->3) S2 space

    2) Old generation (also sometimes called as "tenured space")


S1,
S2 are called as survivor spaces

From space --> S1
To space   --> S2


At anypoint of time one space is empty.

 ~~~~~~~~~~

Eden area ==> for object allocation

+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
|  +--------+  +--------+  +--------+  |
|  | Object |  | Object |  | Object |  |
|  +--------+  +--------+  +--------+  |
|  +--------+  +--------+  +--------+  |
|  | Object |  | Object |  | Object |  |
|  +--------+  +--------+  +--------+  |
|           Eden area is FULL          |
+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+

        when Eden area is FULL
    Objects copied to one of Survivor spaces S1 or S2

If the survivor space contains objects
        --> that means objects have passed atleast one gc.

------>

+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
|        survivor space ***S1***       |
|           survivor is FULL           |
|  +--------+  +--------+  +--------+  |
|  | Object |  | Object |  | Object |  |
|  +--------+  +--------+  +--------+  |
|  +--------+  +--------+  +--------+  |
|  | Object |  | Object |  | Object |  |
|  +--------+  +--------+  +--------+  |
+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+


When the survivor space is FULL,
    surviving objects moved from one space S1 to other space S1


+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
|        survivor space ***S1***       |
|                                      |
|                                      | --> will become destination for
|                                      |     next copy from Eden space
|                                      |
|                                      |
+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+

+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
|        survivor space *******S2******|
|           survivor is FULL           |
|  +--------+  +--------+  +--------+  |
|  | Object |  | Object |  | Object |  |
|  +--------+  +--------+  +--------+  |
|  +--------+  +--------+  +--------+  |
|  | Object |  | Object |  | Object |  |
|  +--------+  +--------+  +--------+  |
+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+



Why Old generation?
-------------------

After certain no. of gcs has passed, but objects still reside in
young generation , ( NOT moved to survivor space) are moved to old generation.
This means objects in the old generations still have references from application
code from or other objects.


Permanent space:
-----------------

+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
|          JVM has                     |
|   internal representation of heap    |
|                                      |
+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+

+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
|          Permanent Space             |
|   internal representation of         |  --> this independant from heap area.
|       JAVA Classes                   |
+~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+

* garbage collection happens in "permanent space" also.
  we can enable or disable by jvm arg "-noclassgc"


Native Area or Code cache:
---------------------------

* JVM internal operations like
        execute JNI
    happens
 in "Native area"

* Size of Native memory
    * depends on operating system.
    * amount of memory already committed to java heap.

* can you find exact size of native memory?
    NO no no no

    yes I did it approximately ( but not excatly)

    Native memory size = Process Size - Max Heap Size - Max Perm Size

Stack:
------
    LIFO ( Last in First Out )

    * Most recently used memory is always removed first.

    * thread stack memory is NOT allocated from heap.
        ( heap size is controlled by -Xmx -Xms )
        -Xms : initial heap size
        -Xmx : max heap size => typically 1/4 of physical memory
                             => if 2 GB ram , then max heap size can be 512 MB

    * stack size is controlled by -Xss

    * utilities find stack info.
        jps
        jstack

what is this memnory equation:
------------------------------

    Heap size
        +
no. of threads * thread stack size
        -
    total RAM used by JVM


linux:
------
    find upper limit of stack setting
    ulimit -s

Generator sizing:
-----------------
    young generation cane be adjusted with

    -XX: NewSize ( split between eden space and survivor space )
                                                    +
                                                    |
                                                    |
                                                    +--> Survivor space tuning
                                                         can be
                                                         -XX: SurvivorRatio = 6

                                                         ( 1/6 of eden space )
    -XX: MaxNewSize


Why keep -Xms and -Xmx size as same?
------------------------------------

* no calculation needed at runtime to extend the initial heap size.


How is default java size is determined?
---------------------------------------

ram/physical memory : upto ~ 194 MB
heap size           : 1/2 of (upto ~ 194 MB)

ram/physical memory : 194mb upto ~  1GB
heap size           : 1/4 th of (194mb upto ~  1GB)

ex:
    1)
     ram/physical memory : 128MB
     heap size           : 1/2 * 128 => 64MB

    2)
     ram/physical memory : 1GB
     heap size           : 1/4 * 1GB => 256MB


Find out default memory model:
------------------------------

java -XX:+PrintFlagsFinal -version | grep "whatever you want"/for ex: heap


                    [Global flags]
                        uintx AdaptiveSizeDecrementScaleFactor          = 4                                   {product}
                        uintx AdaptiveSizeMajorGCDecayTimeScale         = 10                                  {product}
                        uintx AdaptiveSizePausePolicy                   = 0                                   {product}
                        uintx AdaptiveSizePolicyCollectionCostMargin    = 50                                  {product}
                        uintx AdaptiveSizePolicyInitializingSteps       = 20                                  {product}
                        uintx AdaptiveSizePolicyOutputInterval          = 0                                   {product}
                        uintx AdaptiveSizePolicyWeight                  = 10                                  {product}
                        uintx AdaptiveSizeThroughPutPolicy              = 0                                   {product}
                        uintx AdaptiveTimeWeight                        = 25                                  {product}
                         bool AdjustConcurrency                         = false                               {product}
                         bool AggressiveOpts                            = false                               {product}
                         intx AliasLevel                                = 3                                   {C2 product}
                         bool AlignVector                               = false                               {C2 product}
                         intx AllocateInstancePrefetchLines             = 1                                   {product}
                         intx AllocatePrefetchDistance                  = 192                                 {product}
                         intx AllocatePrefetchInstr                     = 0                                   {product}
                         intx AllocatePrefetchLines                     = 4                                   {product}
                         intx AllocatePrefetchStepSize                  = 64                                  {product}
                         intx AllocatePrefetchStyle                     = 1                                   {product}
                         bool AllowJNIEnvProxy                          = false                               {product}
                         bool AllowNonVirtualCalls                      = false                               {product}
                         bool AllowParallelDefineClass                  = false                               {product}
                         bool AllowUserSignalHandlers                   = false                               {product}
                         bool AlwaysActAsServerClassMachine             = false                               {product}
                         bool AlwaysCompileLoopMethods                  = false                               {product}
                         bool AlwaysLockClassLoader                     = false                               {product}
                         bool AlwaysPreTouch                            = false                               {product}
                         bool AlwaysRestoreFPU                          = false                               {product}
                         bool AlwaysTenure                              = false                               {product}
                         bool AssertOnSuspendWaitFailure                = false                               {product}
                         bool AssumeMP                                  = false                               {product}
                         intx AutoBoxCacheMax                           = 128                                 {C2 product}
                        uintx AutoGCSelectPauseMillis                   = 5000                                {product}
                         intx BCEATraceLevel                            = 0                                   {product}
                         intx BackEdgeThreshold                         = 100000                              {pd product}
                         bool BackgroundCompilation                     = true                                {pd product}
                        uintx BaseFootPrintEstimate                     = 268435456                           {product}
                         intx BiasedLockingBulkRebiasThreshold          = 20                                  {product}
                         intx BiasedLockingBulkRevokeThreshold          = 40                                  {product}
                         intx BiasedLockingDecayTime                    = 25000                               {product}
                         intx BiasedLockingStartupDelay                 = 4000                                {product}
                         bool BindGCTaskThreadsToCPUs                   = false                               {product}
                         bool BlockLayoutByFrequency                    = true                                {C2 product}
                         intx BlockLayoutMinDiamondPercentage           = 20                                  {C2 product}
                         bool BlockLayoutRotateLoops                    = true                                {C2 product}
                         bool BranchOnRegister                          = false                               {C2 product}
                         bool BytecodeVerificationLocal                 = false                               {product}
                         bool BytecodeVerificationRemote                = true                                {product}
                         bool C1OptimizeVirtualCallProfiling            = true                                {C1 product}
                         bool C1ProfileBranches                         = true                                {C1 product}
                         bool C1ProfileCalls                            = true                                {C1 product}
                         bool C1ProfileCheckcasts                       = true                                {C1 product}
                         bool C1ProfileInlinedCalls                     = true                                {C1 product}
                         bool C1ProfileVirtualCalls                     = true                                {C1 product}
                         bool C1UpdateMethodData                        = true                                {C1 product}
                         intx CICompilerCount                          := 4                                   {product}
                         bool CICompilerCountPerCPU                     = true                                {product}
                         bool CITime                                    = false                               {product}
                         bool CMSAbortSemantics                         = false                               {product}
                        uintx CMSAbortablePrecleanMinWorkPerIteration   = 100                                 {product}
                         intx CMSAbortablePrecleanWaitMillis            = 100                                 {manageable}
                        uintx CMSBitMapYieldQuantum                     = 10485760                            {product}
                        uintx CMSBootstrapOccupancy                     = 50                                  {product}
                         bool CMSClassUnloadingEnabled                  = true                                {product}
                        uintx CMSClassUnloadingMaxInterval              = 0                                   {product}
                         bool CMSCleanOnEnter                           = true                                {product}
                         bool CMSCompactWhenClearAllSoftRefs            = true                                {product}
                        uintx CMSConcMarkMultiple                       = 32                                  {product}
                         bool CMSConcurrentMTEnabled                    = true                                {product}
                        uintx CMSCoordinatorYieldSleepCount             = 10                                  {product}
                         bool CMSDumpAtPromotionFailure                 = false                               {product}
                         bool CMSEdenChunksRecordAlways                 = true                                {product}
                        uintx CMSExpAvgFactor                           = 50                                  {product}
                         bool CMSExtrapolateSweep                       = false                               {product}
                        uintx CMSFullGCsBeforeCompaction                = 0                                   {product}
                        uintx CMSIncrementalDutyCycle                   = 10                                  {product}
                        uintx CMSIncrementalDutyCycleMin                = 0                                   {product}
                         bool CMSIncrementalMode                        = false                               {product}
                        uintx CMSIncrementalOffset                      = 0                                   {product}
                         bool CMSIncrementalPacing                      = true                                {product}
                        uintx CMSIncrementalSafetyFactor                = 10                                  {product}
                        uintx CMSIndexedFreeListReplenish               = 4                                   {product}
                         intx CMSInitiatingOccupancyFraction            = -1                                  {product}
                        uintx CMSIsTooFullPercentage                    = 98                                  {product}
                       double CMSLargeCoalSurplusPercent                = 0.950000                            {product}
                       double CMSLargeSplitSurplusPercent               = 1.000000                            {product}
                         bool CMSLoopWarn                               = false                               {product}
                        uintx CMSMaxAbortablePrecleanLoops              = 0                                   {product}
                         intx CMSMaxAbortablePrecleanTime               = 5000                                {product}
                        uintx CMSOldPLABMax                             = 1024                                {product}
                        uintx CMSOldPLABMin                             = 16                                  {product}
                        uintx CMSOldPLABNumRefills                      = 4                                   {product}
                        uintx CMSOldPLABReactivityFactor                = 2                                   {product}
                         bool CMSOldPLABResizeQuicker                   = false                               {product}
                        uintx CMSOldPLABToleranceFactor                 = 4                                   {product}
                         bool CMSPLABRecordAlways                       = true                                {product}
                        uintx CMSParPromoteBlocksToClaim                = 16                                  {product}
                         bool CMSParallelInitialMarkEnabled             = true                                {product}
                         bool CMSParallelRemarkEnabled                  = true                                {product}
                         bool CMSParallelSurvivorRemarkEnabled          = true                                {product}
                        uintx CMSPrecleanDenominator                    = 3                                   {product}
                        uintx CMSPrecleanIter                           = 3                                   {product}
                        uintx CMSPrecleanNumerator                      = 2                                   {product}
                         bool CMSPrecleanRefLists1                      = true                                {product}
                         bool CMSPrecleanRefLists2                      = false                               {product}
                         bool CMSPrecleanSurvivors1                     = false                               {product}
                         bool CMSPrecleanSurvivors2                     = true                                {product}
                        uintx CMSPrecleanThreshold                      = 1000                                {product}
                         bool CMSPrecleaningEnabled                     = true                                {product}
                         bool CMSPrintChunksInDump                      = false                               {product}
                         bool CMSPrintEdenSurvivorChunks                = false                               {product}
                         bool CMSPrintObjectsInDump                     = false                               {product}
                        uintx CMSRemarkVerifyVariant                    = 1                                   {product}
                         bool CMSReplenishIntermediate                  = true                                {product}
                        uintx CMSRescanMultiple                         = 32                                  {product}
                        uintx CMSSamplingGrain                          = 16384                               {product}
                         bool CMSScavengeBeforeRemark                   = false                               {product}
                        uintx CMSScheduleRemarkEdenPenetration          = 50                                  {product}
                        uintx CMSScheduleRemarkEdenSizeThreshold        = 2097152                             {product}
                        uintx CMSScheduleRemarkSamplingRatio            = 5                                   {product}
                       double CMSSmallCoalSurplusPercent                = 1.050000                            {product}
                       double CMSSmallSplitSurplusPercent               = 1.100000                            {product}
                         bool CMSSplitIndexedFreeListBlocks             = true                                {product}
                         intx CMSTriggerInterval                        = -1                                  {manageable}
                        uintx CMSTriggerRatio                           = 80                                  {product}
                         intx CMSWaitDuration                           = 2000                                {manageable}
                        uintx CMSWorkQueueDrainThreshold                = 10                                  {product}
                         bool CMSYield                                  = true                                {product}
                        uintx CMSYieldSleepCount                        = 0                                   {product}
                        uintx CMSYoungGenPerWorker                      = 67108864                            {pd product}
                        uintx CMS_FLSPadding                            = 1                                   {product}
                        uintx CMS_FLSWeight                             = 75                                  {product}
                        uintx CMS_SweepPadding                          = 1                                   {product}
                        uintx CMS_SweepTimerThresholdMillis             = 10                                  {product}
                        uintx CMS_SweepWeight                           = 75                                  {product}
                         bool CheckEndorsedAndExtDirs                   = false                               {product}
                         bool CheckJNICalls                             = false                               {product}
                         bool ClassUnloading                            = true                                {product}
                         bool ClassUnloadingWithConcurrentMark          = true                                {product}
                         intx ClearFPUAtPark                            = 0                                   {product}
                         bool ClipInlining                              = true                                {product}
                        uintx CodeCacheExpansionSize                    = 65536                               {pd product}
                        uintx CodeCacheMinimumFreeSpace                 = 512000                              {product}
                         bool CollectGen0First                          = false                               {product}
                         bool CompactFields                             = true                                {product}
                         intx CompilationPolicyChoice                   = 3                                   {product}
                    ccstrlist CompileCommand                            =                                     {product}
                        ccstr CompileCommandFile                        =                                     {product}
                    ccstrlist CompileOnly                               =                                     {product}
                         intx CompileThreshold                          = 10000                               {pd product}
                         bool CompilerThreadHintNoPreempt               = true                                {product}
                         intx CompilerThreadPriority                    = -1                                  {product}
                         intx CompilerThreadStackSize                   = 0                                   {pd product}
                        uintx CompressedClassSpaceSize                  = 1073741824                          {product}
                        uintx ConcGCThreads                             = 0                                   {product}
                         intx ConditionalMoveLimit                      = 3                                   {C2 pd product}
                         intx ContendedPaddingWidth                     = 128                                 {product}
                         bool ConvertSleepToYield                       = true                                {pd product}
                         bool ConvertYieldToSleep                       = false                               {product}
                         bool CreateMinidumpOnCrash                     = false                               {product}
                         bool CriticalJNINatives                        = true                                {product}
                         bool DTraceAllocProbes                         = false                               {product}
                         bool DTraceMethodProbes                        = false                               {product}
                         bool DTraceMonitorProbes                       = false                               {product}
                         bool Debugging                                 = false                               {product}
                        uintx DefaultMaxRAMFraction                     = 4                                   {product}
                         intx DefaultThreadPriority                     = -1                                  {product}
                         intx DeferPollingPageLoopCount                 = -1                                  {product}
                         intx DeferThrSuspendLoopCount                  = 4000                                {product}
                         bool DeoptimizeRandom                          = false                               {product}
                         bool DisableAttachMechanism                    = false                               {product}
                         bool DisableExplicitGC                         = false                               {product}
                         bool DisplayVMOutputToStderr                   = false                               {product}
                         bool DisplayVMOutputToStdout                   = false                               {product}
                         bool DoEscapeAnalysis                          = true                                {C2 product}
                         bool DontCompileHugeMethods                    = true                                {product}
                         bool DontYieldALot                             = false                               {pd product}
                        ccstr DumpLoadedClassList                       =                                     {product}
                         bool DumpReplayDataOnError                     = true                                {product}
                         bool DumpSharedSpaces                          = false                               {product}
                         bool EagerXrunInit                             = false                               {product}
                         intx EliminateAllocationArraySizeLimit         = 64                                  {C2 product}
                         bool EliminateAllocations                      = true                                {C2 product}
                         bool EliminateAutoBox                          = true                                {C2 product}
                         bool EliminateLocks                            = true                                {C2 product}
                         bool EliminateNestedLocks                      = true                                {C2 product}
                         intx EmitSync                                  = 0                                   {product}
                         bool EnableContended                           = true                                {product}
                         bool EnableResourceManagementTLABCache         = true                                {product}
                         bool EnableSharedLookupCache                   = true                                {product}
                         bool EnableTracing                             = false                               {product}
                        uintx ErgoHeapSizeLimit                         = 0                                   {product}
                        ccstr ErrorFile                                 =                                     {product}
                        ccstr ErrorReportServer                         =                                     {product}
                       double EscapeAnalysisTimeout                     = 20.000000                           {C2 product}
                         bool EstimateArgEscape                         = true                                {product}
                         bool ExplicitGCInvokesConcurrent               = false                               {product}
                         bool ExplicitGCInvokesConcurrentAndUnloadsClasses  = false                               {product}
                         bool ExtendedDTraceProbes                      = false                               {product}
                        ccstr ExtraSharedClassListFile                  =                                     {product}
                         bool FLSAlwaysCoalesceLarge                    = false                               {product}
                        uintx FLSCoalescePolicy                         = 2                                   {product}
                       double FLSLargestBlockCoalesceProximity          = 0.990000                            {product}
                         bool FailOverToOldVerifier                     = true                                {product}
                         bool FastTLABRefill                            = true                                {product}
                         intx FenceInstruction                          = 0                                   {ARCH product}
                         intx FieldsAllocationStyle                     = 1                                   {product}
                         bool FilterSpuriousWakeups                     = true                                {product}
                        ccstr FlightRecorderOptions                     =                                     {product}
                         bool ForceNUMA                                 = false                               {product}
                         bool ForceTimeHighResolution                   = false                               {product}
                         intx FreqInlineSize                            = 325                                 {pd product}
                       double G1ConcMarkStepDurationMillis              = 10.000000                           {product}
                        uintx G1ConcRSHotCardLimit                      = 4                                   {product}
                        uintx G1ConcRSLogCacheSize                      = 10                                  {product}
                         intx G1ConcRefinementGreenZone                 = 0                                   {product}
                         intx G1ConcRefinementRedZone                   = 0                                   {product}
                         intx G1ConcRefinementServiceIntervalMillis     = 300                                 {product}
                        uintx G1ConcRefinementThreads                   = 0                                   {product}
                         intx G1ConcRefinementThresholdStep             = 0                                   {product}
                         intx G1ConcRefinementYellowZone                = 0                                   {product}
                        uintx G1ConfidencePercent                       = 50                                  {product}
                        uintx G1HeapRegionSize                          = 0                                   {product}
                        uintx G1HeapWastePercent                        = 5                                   {product}
                        uintx G1MixedGCCountTarget                      = 8                                   {product}
                         intx G1RSetRegionEntries                       = 0                                   {product}
                        uintx G1RSetScanBlockSize                       = 64                                  {product}
                         intx G1RSetSparseRegionEntries                 = 0                                   {product}
                         intx G1RSetUpdatingPauseTimePercent            = 10                                  {product}
                         intx G1RefProcDrainInterval                    = 10                                  {product}
                        uintx G1ReservePercent                          = 10                                  {product}
                        uintx G1SATBBufferEnqueueingThresholdPercent    = 60                                  {product}
                         intx G1SATBBufferSize                          = 1024                                {product}
                         intx G1UpdateBufferSize                        = 256                                 {product}
                         bool G1UseAdaptiveConcRefinement               = true                                {product}
                        uintx GCDrainStackTargetSize                    = 64                                  {product}
                        uintx GCHeapFreeLimit                           = 2                                   {product}
                        uintx GCLockerEdenExpansionPercent              = 5                                   {product}
                         bool GCLockerInvokesConcurrent                 = false                               {product}
                        uintx GCLogFileSize                             = 8192                                {product}
                        uintx GCPauseIntervalMillis                     = 0                                   {product}
                        uintx GCTaskTimeStampEntries                    = 200                                 {product}
                        uintx GCTimeLimit                               = 98                                  {product}
                        uintx GCTimeRatio                               = 99                                  {product}
                        uintx HeapBaseMinAddress                        = 2147483648                          {pd product}
                         bool HeapDumpAfterFullGC                       = false                               {manageable}
                         bool HeapDumpBeforeFullGC                      = false                               {manageable}
                         bool HeapDumpOnOutOfMemoryError                = false                               {manageable}
                        ccstr HeapDumpPath                              =                                     {manageable}
                        uintx HeapFirstMaximumCompactionCount           = 3                                   {product}
                        uintx HeapMaximumCompactionInterval             = 20                                  {product}
                        uintx HeapSizePerGCThread                       = 87241520                            {product}
                         bool IgnoreEmptyClassPaths                     = false                               {product}
                         bool IgnoreUnrecognizedVMOptions               = false                               {product}
                        uintx IncreaseFirstTierCompileThresholdAt       = 50                                  {product}
                         bool IncrementalInline                         = true                                {C2 product}
                        uintx InitialBootClassLoaderMetaspaceSize       = 4194304                             {product}
                        uintx InitialCodeCacheSize                      = 2555904                             {pd product}
                        uintx InitialHeapSize                          := 132120576                           {product}
                        uintx InitialRAMFraction                        = 64                                  {product}
                        uintx InitialSurvivorRatio                      = 8                                   {product}
                        uintx InitialTenuringThreshold                  = 7                                   {product}
                        uintx InitiatingHeapOccupancyPercent            = 45                                  {product}
                         bool Inline                                    = true                                {product}
                        ccstr InlineDataFile                            =                                     {product}
                         intx InlineSmallCode                           = 2000                                {pd product}
                         bool InlineSynchronizedMethods                 = true                                {C1 product}
                         bool InsertMemBarAfterArraycopy                = true                                {C2 product}
                         intx InteriorEntryAlignment                    = 16                                  {C2 pd product}
                         intx InterpreterProfilePercentage              = 33                                  {product}
                         bool JNIDetachReleasesMonitors                 = true                                {product}
                         bool JavaMonitorsInStackTrace                  = true                                {product}
                         intx JavaPriority10_To_OSPriority              = -1                                  {product}
                         intx JavaPriority1_To_OSPriority               = -1                                  {product}
                         intx JavaPriority2_To_OSPriority               = -1                                  {product}
                         intx JavaPriority3_To_OSPriority               = -1                                  {product}
                         intx JavaPriority4_To_OSPriority               = -1                                  {product}
                         intx JavaPriority5_To_OSPriority               = -1                                  {product}
                         intx JavaPriority6_To_OSPriority               = -1                                  {product}
                         intx JavaPriority7_To_OSPriority               = -1                                  {product}
                         intx JavaPriority8_To_OSPriority               = -1                                  {product}
                         intx JavaPriority9_To_OSPriority               = -1                                  {product}
                         bool LIRFillDelaySlots                         = false                               {C1 pd product}
                        uintx LargePageHeapSizeThreshold                = 134217728                           {product}
                        uintx LargePageSizeInBytes                      = 0                                   {product}
                         bool LazyBootClassLoader                       = true                                {product}
                         intx LiveNodeCountInliningCutoff               = 40000                               {C2 product}
                         bool LogCommercialFeatures                     = false                               {product}
                         intx LoopMaxUnroll                             = 16                                  {C2 product}
                         intx LoopOptsCount                             = 43                                  {C2 product}
                         intx LoopUnrollLimit                           = 60                                  {C2 pd product}
                         intx LoopUnrollMin                             = 4                                   {C2 product}
                         bool LoopUnswitching                           = true                                {C2 product}
                         bool ManagementServer                          = false                               {product}
                        uintx MarkStackSize                             = 4194304                             {product}
                        uintx MarkStackSizeMax                          = 536870912                           {product}
                        uintx MarkSweepAlwaysCompactCount               = 4                                   {product}
                        uintx MarkSweepDeadRatio                        = 1                                   {product}
                         intx MaxBCEAEstimateLevel                      = 5                                   {product}
                         intx MaxBCEAEstimateSize                       = 150                                 {product}
                        uintx MaxDirectMemorySize                       = 0                                   {product}
                         bool MaxFDLimit                                = true                                {product}
                        uintx MaxGCMinorPauseMillis                     = 4294967295                          {product}
                        uintx MaxGCPauseMillis                          = 4294967295                          {product}
                        uintx MaxHeapFreeRatio                          = 100                                 {manageable}
                        uintx MaxHeapSize                              := 2101346304                          {product}
                         intx MaxInlineLevel                            = 9                                   {product}
                         intx MaxInlineSize                             = 35                                  {product}
                         intx MaxJNILocalCapacity                       = 65536                               {product}
                         intx MaxJavaStackTraceDepth                    = 1024                                {product}
                         intx MaxJumpTableSize                          = 65000                               {C2 product}
                         intx MaxJumpTableSparseness                    = 5                                   {C2 product}
                         intx MaxLabelRootDepth                         = 1100                                {C2 product}
                         intx MaxLoopPad                                = 11                                  {C2 product}
                        uintx MaxMetaspaceExpansion                     = 5451776                             {product}
                        uintx MaxMetaspaceFreeRatio                     = 70                                  {product}
                        uintx MaxMetaspaceSize                          = 4294901760                          {product}
                        uintx MaxNewSize                               := 700448768                           {product}
                         intx MaxNodeLimit                              = 75000                               {C2 product}
                     uint64_t MaxRAM                                    = 0                                   {pd product}
                        uintx MaxRAMFraction                            = 4                                   {product}
                         intx MaxRecursiveInlineLevel                   = 1                                   {product}
                        uintx MaxTenuringThreshold                      = 15                                  {product}
                         intx MaxTrivialSize                            = 6                                   {product}
                         intx MaxVectorSize                             = 32                                  {C2 product}
                        uintx MetaspaceSize                             = 21807104                            {pd product}
                         bool MethodFlushing                            = true                                {product}
                        uintx MinHeapDeltaBytes                        := 524288                              {product}
                        uintx MinHeapFreeRatio                          = 0                                   {manageable}
                         intx MinInliningThreshold                      = 250                                 {product}
                         intx MinJumpTableSize                          = 10                                  {C2 pd product}
                        uintx MinMetaspaceExpansion                     = 339968                              {product}
                        uintx MinMetaspaceFreeRatio                     = 40                                  {product}
                        uintx MinRAMFraction                            = 2                                   {product}
                        uintx MinSurvivorRatio                          = 3                                   {product}
                        uintx MinTLABSize                               = 2048                                {product}
                         intx MonitorBound                              = 0                                   {product}
                         bool MonitorInUseLists                         = false                               {product}
                         intx MultiArrayExpandLimit                     = 6                                   {C2 product}
                         bool MustCallLoadClassInternal                 = false                               {product}
                        uintx NUMAChunkResizeWeight                     = 20                                  {product}
                        uintx NUMAInterleaveGranularity                 = 2097152                             {product}
                        uintx NUMAPageScanRate                          = 256                                 {product}
                        uintx NUMASpaceResizeRate                       = 1073741824                          {product}
                         bool NUMAStats                                 = false                               {product}
                        ccstr NativeMemoryTracking                      = off                                 {product}
                         bool NeedsDeoptSuspend                         = false                               {pd product}
                         bool NeverActAsServerClassMachine              = false                               {pd product}
                         bool NeverTenure                               = false                               {product}
                        uintx NewRatio                                  = 2                                   {product}
                        uintx NewSize                                  := 44040192                            {product}
                        uintx NewSizeThreadIncrease                     = 5320                                {pd product}
                         intx NmethodSweepActivity                      = 10                                  {product}
                         intx NmethodSweepCheckInterval                 = 5                                   {product}
                         intx NmethodSweepFraction                      = 16                                  {product}
                         intx NodeLimitFudgeFactor                      = 2000                                {C2 product}
                        uintx NumberOfGCLogFiles                        = 0                                   {product}
                         intx NumberOfLoopInstrToAlign                  = 4                                   {C2 product}
                         intx ObjectAlignmentInBytes                    = 8                                   {lp64_product}
                        uintx OldPLABSize                               = 1024                                {product}
                        uintx OldPLABWeight                             = 50                                  {product}
                        uintx OldSize                                  := 88080384                            {product}
                         bool OmitStackTraceInFastThrow                 = true                                {product}
                    ccstrlist OnError                                   =                                     {product}
                    ccstrlist OnOutOfMemoryError                        =                                     {product}
                         intx OnStackReplacePercentage                  = 140                                 {pd product}
                         bool OptimizeFill                              = true                                {C2 product}
                         bool OptimizePtrCompare                        = true                                {C2 product}
                         bool OptimizeStringConcat                      = true                                {C2 product}
                         bool OptoBundling                              = false                               {C2 pd product}
                         intx OptoLoopAlignment                         = 16                                  {pd product}
                         bool OptoScheduling                            = false                               {C2 pd product}
                        uintx PLABWeight                                = 75                                  {product}
                         bool PSChunkLargeArrays                        = true                                {product}
                         intx ParGCArrayScanChunk                       = 50                                  {product}
                        uintx ParGCDesiredObjsFromOverflowList          = 20                                  {product}
                         bool ParGCTrimOverflow                         = true                                {product}
                         bool ParGCUseLocalOverflow                     = false                               {product}
                        uintx ParallelGCBufferWastePct                  = 10                                  {product}
                        uintx ParallelGCThreads                         = 8                                   {product}
                         bool ParallelGCVerbose                         = false                               {product}
                        uintx ParallelOldDeadWoodLimiterMean            = 50                                  {product}
                        uintx ParallelOldDeadWoodLimiterStdDev          = 80                                  {product}
                         bool ParallelRefProcBalancingEnabled           = true                                {product}
                         bool ParallelRefProcEnabled                    = false                               {product}
                         bool PartialPeelAtUnsignedTests                = true                                {C2 product}
                         bool PartialPeelLoop                           = true                                {C2 product}
                         intx PartialPeelNewPhiDelta                    = 0                                   {C2 product}
                        uintx PausePadding                              = 1                                   {product}
                         intx PerBytecodeRecompilationCutoff            = 200                                 {product}
                         intx PerBytecodeTrapLimit                      = 4                                   {product}
                         intx PerMethodRecompilationCutoff              = 400                                 {product}
                         intx PerMethodTrapLimit                        = 100                                 {product}
                         bool PerfAllowAtExitRegistration               = false                               {product}
                         bool PerfBypassFileSystemCheck                 = false                               {product}
                         intx PerfDataMemorySize                        = 32768                               {product}
                         intx PerfDataSamplingInterval                  = 50                                  {product}
                        ccstr PerfDataSaveFile                          =                                     {product}
                         bool PerfDataSaveToFile                        = false                               {product}
                         bool PerfDisableSharedMem                      = false                               {product}
                         intx PerfMaxStringConstLength                  = 1024                                {product}
                         intx PreInflateSpin                            = 10                                  {pd product}
                         bool PreferInterpreterNativeStubs              = false                               {pd product}
                         intx PrefetchCopyIntervalInBytes               = 576                                 {product}
                         intx PrefetchFieldsAhead                       = 1                                   {product}
                         intx PrefetchScanIntervalInBytes               = 576                                 {product}
                         bool PreserveAllAnnotations                    = false                               {product}
                         bool PreserveFramePointer                      = false                               {pd product}
                        uintx PretenureSizeThreshold                    = 0                                   {product}
                         bool PrintAdaptiveSizePolicy                   = false                               {product}
                         bool PrintCMSInitiationStatistics              = false                               {product}
                         intx PrintCMSStatistics                        = 0                                   {product}
                         bool PrintClassHistogram                       = false                               {manageable}
                         bool PrintClassHistogramAfterFullGC            = false                               {manageable}
                         bool PrintClassHistogramBeforeFullGC           = false                               {manageable}
                         bool PrintCodeCache                            = false                               {product}
                         bool PrintCodeCacheOnCompilation               = false                               {product}
                         bool PrintCommandLineFlags                     = false                               {product}
                         bool PrintCompilation                          = false                               {product}
                         bool PrintConcurrentLocks                      = false                               {manageable}
                         intx PrintFLSCensus                            = 0                                   {product}
                         intx PrintFLSStatistics                        = 0                                   {product}
                         bool PrintFlagsFinal                          := true                                {product}
                         bool PrintFlagsInitial                         = false                               {product}
                         bool PrintGC                                   = false                               {manageable}
                         bool PrintGCApplicationConcurrentTime          = false                               {product}
                         bool PrintGCApplicationStoppedTime             = false                               {product}
                         bool PrintGCCause                              = true                                {product}
                         bool PrintGCDateStamps                         = false                               {manageable}
                         bool PrintGCDetails                            = false                               {manageable}
                         bool PrintGCID                                 = false                               {manageable}
                         bool PrintGCTaskTimeStamps                     = false                               {product}
                         bool PrintGCTimeStamps                         = false                               {manageable}
                         bool PrintHeapAtGC                             = false                               {product rw}
                         bool PrintHeapAtGCExtended                     = false                               {product rw}
                         bool PrintHeapAtSIGBREAK                       = true                                {product}
                         bool PrintJNIGCStalls                          = false                               {product}
                         bool PrintJNIResolving                         = false                               {product}
                         bool PrintOldPLAB                              = false                               {product}
                         bool PrintOopAddress                           = false                               {product}
                         bool PrintPLAB                                 = false                               {product}
                         bool PrintParallelOldGCPhaseTimes              = false                               {product}
                         bool PrintPromotionFailure                     = false                               {product}
                         bool PrintReferenceGC                          = false                               {product}
                         bool PrintSafepointStatistics                  = false                               {product}
                         intx PrintSafepointStatisticsCount             = 300                                 {product}
                         intx PrintSafepointStatisticsTimeout           = -1                                  {product}
                         bool PrintSharedArchiveAndExit                 = false                               {product}
                         bool PrintSharedDictionary                     = false                               {product}
                         bool PrintSharedSpaces                         = false                               {product}
                         bool PrintStringDeduplicationStatistics        = false                               {product}
                         bool PrintStringTableStatistics                = false                               {product}
                         bool PrintTLAB                                 = false                               {product}
                         bool PrintTenuringDistribution                 = false                               {product}
                         bool PrintTieredEvents                         = false                               {product}
                         bool PrintVMOptions                            = false                               {product}
                         bool PrintVMQWaitTime                          = false                               {product}
                         bool PrintWarnings                             = true                                {product}
                        uintx ProcessDistributionStride                 = 4                                   {product}
                         bool ProfileInterpreter                        = true                                {pd product}
                         bool ProfileIntervals                          = false                               {product}
                         intx ProfileIntervalsTicks                     = 100                                 {product}
                         intx ProfileMaturityPercentage                 = 20                                  {product}
                         bool ProfileVM                                 = false                               {product}
                         bool ProfilerPrintByteCodeStatistics           = false                               {product}
                         bool ProfilerRecordPC                          = false                               {product}
                        uintx PromotedPadding                           = 3                                   {product}
                        uintx QueuedAllocationWarningCount              = 0                                   {product}
                        uintx RTMRetryCount                             = 5                                   {ARCH product}
                         bool RangeCheckElimination                     = true                                {product}
                         intx ReadPrefetchInstr                         = 0                                   {ARCH product}
                         bool ReassociateInvariants                     = true                                {C2 product}
                         bool ReduceBulkZeroing                         = true                                {C2 product}
                         bool ReduceFieldZeroing                        = true                                {C2 product}
                         bool ReduceInitialCardMarks                    = true                                {C2 product}
                         bool ReduceSignalUsage                         = false                               {product}
                         intx RefDiscoveryPolicy                        = 0                                   {product}
                         bool ReflectionWrapResolutionErrors            = true                                {product}
                         bool RegisterFinalizersAtInit                  = true                                {product}
                         bool RelaxAccessControlCheck                   = false                               {product}
                        ccstr ReplayDataFile                            =                                     {product}
                         bool RequireSharedSpaces                       = false                               {product}
                        uintx ReservedCodeCacheSize                     = 251658240                           {pd product}
                         bool ResizeOldPLAB                             = true                                {product}
                         bool ResizePLAB                                = true                                {product}
                         bool ResizeTLAB                                = true                                {pd product}
                         bool RestoreMXCSROnJNICalls                    = false                               {product}
                         bool RestrictContended                         = true                                {product}
                         bool RewriteBytecodes                          = true                                {pd product}
                         bool RewriteFrequentPairs                      = true                                {pd product}
                         intx SafepointPollOffset                       = 256                                 {C1 pd product}
                         intx SafepointSpinBeforeYield                  = 2000                                {product}
                         bool SafepointTimeout                          = false                               {product}
                         intx SafepointTimeoutDelay                     = 10000                               {product}
                         bool ScavengeBeforeFullGC                      = true                                {product}
                         intx SelfDestructTimer                         = 0                                   {product}
                        uintx SharedBaseAddress                         = 0                                   {product}
                        ccstr SharedClassListFile                       =                                     {product}
                        uintx SharedMiscCodeSize                        = 122880                              {product}
                        uintx SharedMiscDataSize                        = 4194304                             {product}
                        uintx SharedReadOnlySize                        = 16777216                            {product}
                        uintx SharedReadWriteSize                       = 16777216                            {product}
                         bool ShowMessageBoxOnError                     = false                               {product}
                         intx SoftRefLRUPolicyMSPerMB                   = 1000                                {product}
                         bool SpecialEncodeISOArray                     = true                                {C2 product}
                         bool SplitIfBlocks                             = true                                {C2 product}
                         intx StackRedPages                             = 1                                   {pd product}
                         intx StackShadowPages                          = 6                                   {pd product}
                         bool StackTraceInThrowable                     = true                                {product}
                         intx StackYellowPages                          = 3                                   {pd product}
                         bool StartAttachListener                       = false                               {product}
                         intx StarvationMonitorInterval                 = 200                                 {product}
                         bool StressLdcRewrite                          = false                               {product}
                        uintx StringDeduplicationAgeThreshold           = 3                                   {product}
                        uintx StringTableSize                           = 60013                               {product}
                         bool SuppressFatalErrorMessage                 = false                               {product}
                        uintx SurvivorPadding                           = 3                                   {product}
                        uintx SurvivorRatio                             = 8                                   {product}
                         intx SuspendRetryCount                         = 50                                  {product}
                         intx SuspendRetryDelay                         = 5                                   {product}
                         intx SyncFlags                                 = 0                                   {product}
                        ccstr SyncKnobs                                 =                                     {product}
                         intx SyncVerbose                               = 0                                   {product}
                        uintx TLABAllocationWeight                      = 35                                  {product}
                        uintx TLABRefillWasteFraction                   = 64                                  {product}
                        uintx TLABSize                                  = 0                                   {product}
                         bool TLABStats                                 = true                                {product}
                        uintx TLABWasteIncrement                        = 4                                   {product}
                        uintx TLABWasteTargetPercent                    = 1                                   {product}
                        uintx TargetPLABWastePct                        = 10                                  {product}
                        uintx TargetSurvivorRatio                       = 50                                  {product}
                        uintx TenuredGenerationSizeIncrement            = 20                                  {product}
                        uintx TenuredGenerationSizeSupplement           = 80                                  {product}
                        uintx TenuredGenerationSizeSupplementDecay      = 2                                   {product}
                         intx ThreadPriorityPolicy                      = 0                                   {product}
                         bool ThreadPriorityVerbose                     = false                               {product}
                        uintx ThreadSafetyMargin                        = 52428800                            {product}
                         intx ThreadStackSize                           = 0                                   {pd product}
                        uintx ThresholdTolerance                        = 10                                  {product}
                         intx Tier0BackedgeNotifyFreqLog                = 10                                  {product}
                         intx Tier0InvokeNotifyFreqLog                  = 7                                   {product}
                         intx Tier0ProfilingStartPercentage             = 200                                 {product}
                         intx Tier23InlineeNotifyFreqLog                = 20                                  {product}
                         intx Tier2BackEdgeThreshold                    = 0                                   {product}
                         intx Tier2BackedgeNotifyFreqLog                = 14                                  {product}
                         intx Tier2CompileThreshold                     = 0                                   {product}
                         intx Tier2InvokeNotifyFreqLog                  = 11                                  {product}
                         intx Tier3BackEdgeThreshold                    = 60000                               {product}
                         intx Tier3BackedgeNotifyFreqLog                = 13                                  {product}
                         intx Tier3CompileThreshold                     = 2000                                {product}
                         intx Tier3DelayOff                             = 2                                   {product}
                         intx Tier3DelayOn                              = 5                                   {product}
                         intx Tier3InvocationThreshold                  = 200                                 {product}
                         intx Tier3InvokeNotifyFreqLog                  = 10                                  {product}
                         intx Tier3LoadFeedback                         = 5                                   {product}
                         intx Tier3MinInvocationThreshold               = 100                                 {product}
                         intx Tier4BackEdgeThreshold                    = 40000                               {product}
                         intx Tier4CompileThreshold                     = 15000                               {product}
                         intx Tier4InvocationThreshold                  = 5000                                {product}
                         intx Tier4LoadFeedback                         = 3                                   {product}
                         intx Tier4MinInvocationThreshold               = 600                                 {product}
                         bool TieredCompilation                         = true                                {pd product}
                         intx TieredCompileTaskTimeout                  = 50                                  {product}
                         intx TieredRateUpdateMaxTime                   = 25                                  {product}
                         intx TieredRateUpdateMinTime                   = 1                                   {product}
                         intx TieredStopAtLevel                         = 4                                   {product}
                         bool TimeLinearScan                            = false                               {C1 product}
                         bool TraceBiasedLocking                        = false                               {product}
                         bool TraceClassLoading                         = false                               {product rw}
                         bool TraceClassLoadingPreorder                 = false                               {product}
                         bool TraceClassPaths                           = false                               {product}
                         bool TraceClassResolution                      = false                               {product}
                         bool TraceClassUnloading                       = false                               {product rw}
                         bool TraceDynamicGCThreads                     = false                               {product}
                         bool TraceGen0Time                             = false                               {product}
                         bool TraceGen1Time                             = false                               {product}
                        ccstr TraceJVMTI                                =                                     {product}
                         bool TraceLoaderConstraints                    = false                               {product rw}
                         bool TraceMetadataHumongousAllocation          = false                               {product}
                         bool TraceMonitorInflation                     = false                               {product}
                         bool TraceParallelOldGCTasks                   = false                               {product}
                         intx TraceRedefineClasses                      = 0                                   {product}
                         bool TraceSafepointCleanupTime                 = false                               {product}
                         bool TraceSharedLookupCache                    = false                               {product}
                         bool TraceSuspendWaitFailures                  = false                               {product}
                         intx TrackedInitializationLimit                = 50                                  {C2 product}
                         bool TransmitErrorReport                       = false                               {product}
                         bool TrapBasedNullChecks                       = false                               {pd product}
                         bool TrapBasedRangeChecks                      = false                               {C2 pd product}
                         intx TypeProfileArgsLimit                      = 2                                   {product}
                        uintx TypeProfileLevel                          = 111                                 {pd product}
                         intx TypeProfileMajorReceiverPercent           = 90                                  {C2 product}
                         intx TypeProfileParmsLimit                     = 2                                   {product}
                         intx TypeProfileWidth                          = 2                                   {product}
                         intx UnguardOnExecutionViolation               = 0                                   {product}
                         bool UnlinkSymbolsALot                         = false                               {product}
                         bool Use486InstrsOnly                          = false                               {ARCH product}
                         bool UseAES                                    = true                                {product}
                         bool UseAESIntrinsics                          = true                                {product}
                         intx UseAVX                                    = 1                                   {ARCH product}
                         bool UseAdaptiveGCBoundary                     = false                               {product}
                         bool UseAdaptiveGenerationSizePolicyAtMajorCollection  = true                                {product}
                         bool UseAdaptiveGenerationSizePolicyAtMinorCollection  = true                                {product}
                         bool UseAdaptiveNUMAChunkSizing                = true                                {product}
                         bool UseAdaptiveSizeDecayMajorGCCost           = true                                {product}
                         bool UseAdaptiveSizePolicy                     = true                                {product}
                         bool UseAdaptiveSizePolicyFootprintGoal        = true                                {product}
                         bool UseAdaptiveSizePolicyWithSystemGC         = false                               {product}
                         bool UseAddressNop                             = true                                {ARCH product}
                         bool UseAltSigs                                = false                               {product}
                         bool UseAutoGCSelectPolicy                     = false                               {product}
                         bool UseBMI1Instructions                       = false                               {ARCH product}
                         bool UseBMI2Instructions                       = false                               {ARCH product}
                         bool UseBiasedLocking                          = true                                {product}
                         bool UseBimorphicInlining                      = true                                {C2 product}
                         bool UseBoundThreads                           = true                                {product}
                         bool UseCLMUL                                  = true                                {ARCH product}
                         bool UseCMSBestFit                             = true                                {product}
                         bool UseCMSCollectionPassing                   = true                                {product}
                         bool UseCMSCompactAtFullCollection             = true                                {product}
                         bool UseCMSInitiatingOccupancyOnly             = false                               {product}
                         bool UseCRC32Intrinsics                        = true                                {product}
                         bool UseCodeCacheFlushing                      = true                                {product}
                         bool UseCompiler                               = true                                {product}
                         bool UseCompilerSafepoints                     = true                                {product}
                         bool UseCompressedClassPointers               := true                                {lp64_product}
                         bool UseCompressedOops                        := true                                {lp64_product}
                         bool UseConcMarkSweepGC                        = false                               {product}
                         bool UseCondCardMark                           = false                               {C2 product}
                         bool UseCountLeadingZerosInstruction           = false                               {ARCH product}
                         bool UseCountTrailingZerosInstruction          = false                               {ARCH product}
                         bool UseCounterDecay                           = true                                {product}
                         bool UseDivMod                                 = true                                {C2 product}
                         bool UseDynamicNumberOfGCThreads               = false                               {product}
                         bool UseFPUForSpilling                         = true                                {C2 product}
                         bool UseFastAccessorMethods                    = false                               {product}
                         bool UseFastEmptyMethods                       = false                               {product}
                         bool UseFastJNIAccessors                       = true                                {product}
                         bool UseFastStosb                              = true                                {ARCH product}
                         bool UseG1GC                                   = false                               {product}
                         bool UseGCLogFileRotation                      = false                               {product}
                         bool UseGCOverheadLimit                        = true                                {product}
                         bool UseGCTaskAffinity                         = false                               {product}
                         bool UseHeavyMonitors                          = false                               {product}
                         bool UseInlineCaches                           = true                                {product}
                         bool UseInterpreter                            = true                                {product}
                         bool UseJumpTables                             = true                                {C2 product}
                         bool UseLWPSynchronization                     = true                                {product}
                         bool UseLargePages                             = false                               {pd product}
                         bool UseLargePagesInMetaspace                  = false                               {product}
                         bool UseLargePagesIndividualAllocation        := false                               {pd product}
                         bool UseLockedTracing                          = false                               {product}
                         bool UseLoopCounter                            = true                                {product}
                         bool UseLoopInvariantCodeMotion                = true                                {C1 product}
                         bool UseLoopPredicate                          = true                                {C2 product}
                         bool UseMathExactIntrinsics                    = true                                {C2 product}
                         bool UseMaximumCompactionOnSystemGC            = true                                {product}
                         bool UseMembar                                 = false                               {pd product}
                         bool UseMultiplyToLenIntrinsic                 = true                                {C2 product}
                         bool UseNUMA                                   = false                               {product}
                         bool UseNUMAInterleaving                       = false                               {product}
                         bool UseNewLongLShift                          = false                               {ARCH product}
                         bool UseOSErrorReporting                       = false                               {pd product}
                         bool UseOldInlining                            = true                                {C2 product}
                         bool UseOnStackReplacement                     = true                                {pd product}
                         bool UseOnlyInlinedBimorphic                   = true                                {C2 product}
                         bool UseOptoBiasInlining                       = true                                {C2 product}
                         bool UsePSAdaptiveSurvivorSizePolicy           = true                                {product}
                         bool UseParNewGC                               = false                               {product}
                         bool UseParallelGC                            := true                                {product}
                         bool UseParallelOldGC                          = true                                {product}
                         bool UsePerfData                               = true                                {product}
                         bool UsePopCountInstruction                    = true                                {product}
                         bool UseRDPCForConstantTableBase               = false                               {C2 product}
                         bool UseRTMDeopt                               = false                               {ARCH product}
                         bool UseRTMLocking                             = false                               {ARCH product}
                         bool UseSHA                                    = false                               {product}
                         bool UseSHA1Intrinsics                         = false                               {product}
                         bool UseSHA256Intrinsics                       = false                               {product}
                         bool UseSHA512Intrinsics                       = false                               {product}
                         intx UseSSE                                    = 4                                   {product}
                         bool UseSSE42Intrinsics                        = true                                {product}
                         bool UseSerialGC                               = false                               {product}
                         bool UseSharedSpaces                           = false                               {product}
                         bool UseSignalChaining                         = true                                {product}
                         bool UseStoreImmI16                            = false                               {ARCH product}
                         bool UseStringDeduplication                    = false                               {product}
                         bool UseSuperWord                              = true                                {C2 product}
                         bool UseTLAB                                   = true                                {pd product}
                         bool UseThreadPriorities                       = true                                {pd product}
                         bool UseTypeProfile                            = true                                {product}
                         bool UseTypeSpeculation                        = true                                {C2 product}
                         bool UseUTCFileTimestamp                       = true                                {product}
                         bool UseUnalignedLoadStores                    = true                                {ARCH product}
                         bool UseVMInterruptibleIO                      = false                               {product}
                         bool UseXMMForArrayCopy                        = true                                {product}
                         bool UseXmmI2D                                 = false                               {ARCH product}
                         bool UseXmmI2F                                 = false                               {ARCH product}
                         bool UseXmmLoadAndClearUpper                   = true                                {ARCH product}
                         bool UseXmmRegToRegMoveAll                     = true                                {ARCH product}
                         bool VMThreadHintNoPreempt                     = false                               {product}
                         intx VMThreadPriority                          = -1                                  {product}
                         intx VMThreadStackSize                         = 0                                   {pd product}
                         intx ValueMapInitialSize                       = 11                                  {C1 product}
                         intx ValueMapMaxLoopSize                       = 8                                   {C1 product}
                         intx ValueSearchLimit                          = 1000                                {C2 product}
                         bool VerifyMergedCPBytecodes                   = true                                {product}
                         bool VerifySharedSpaces                        = false                               {product}
                         intx WorkAroundNPTLTimedWaitHang               = 1                                   {product}
                        uintx YoungGenerationSizeIncrement              = 20                                  {product}
                        uintx YoungGenerationSizeSupplement             = 80                                  {product}
                        uintx YoungGenerationSizeSupplementDecay        = 8                                   {product}
                        uintx YoungPLABSize                             = 4096                                {product}
                         bool ZeroTLAB                                  = false                               {product}
                         intx hashCode                                  = 5                                   {product}
                    java version "1.8.0_91"
                    Java(TM) SE Runtime Environment (build 1.8.0_91-b15)
                    Java HotSpot(TM) 64-Bit Server VM (build 25.91-b15, mixed mode)

----------------------------------------

find out heap details of Java process
( Please note down it has to be "java" process, not even "javaw" process can
be attached.)

Usage: jmap -heap 12464

        jmap -heap 12464
        Attaching to process ID 12464, please wait...
        Debugger attached successfully.
        Server compiler detected.
        JVM version is 25.91-b15

        using thread-local object allocation.
        Parallel GC with 8 thread(s)

        Heap Configuration:
           MinHeapFreeRatio         = 0
           MaxHeapFreeRatio         = 100
           MaxHeapSize              = 2101346304 (2004.0MB)
           NewSize                  = 44040192 (42.0MB)
           MaxNewSize               = 700448768 (668.0MB)
           OldSize                  = 88080384 (84.0MB)
           NewRatio                 = 2
           SurvivorRatio            = 8
           MetaspaceSize            = 21807104 (20.796875MB)
           CompressedClassSpaceSize = 1073741824 (1024.0MB)
           MaxMetaspaceSize         = 17592186044415 MB
           G1HeapRegionSize         = 0 (0.0MB)

        Heap Usage:
        PS Young Generation
        Eden Space:
           capacity = 33554432 (32.0MB)
           used     = 2013328 (1.9200592041015625MB)
           free     = 31541104 (30.079940795898438MB)
           6.000185012817383% used
        From Space:
           capacity = 5242880 (5.0MB)
           used     = 0 (0.0MB)
           free     = 5242880 (5.0MB)
           0.0% used
        To Space:
           capacity = 5242880 (5.0MB)
           used     = 0 (0.0MB)
           free     = 5242880 (5.0MB)
           0.0% used
        PS Old Generation
           capacity = 88080384 (84.0MB)
           used     = 0 (0.0MB)
           free     = 88080384 (84.0MB)
           0.0% used

        1580 interned Strings occupying 121536 bytes.



--------------------------------------------------------------------------------

java -X lists non-standard options:
===================================
java -X
    -Xmixed           混合モードの実行(デフォルト)
    -Xint             インタプリタ・モードの実行のみ
    -Xbootclasspath:<;で区切られたディレクトリおよびzip/jarファイル>
                      ブートストラップのクラスとリソースの検索パスを設定する
    -Xbootclasspath/a:<;で区切られたディレクトリおよびzip/jarファイル>
                      ブートストラップ・クラス・パスの最後に追加する
    -Xbootclasspath/p:<;で区切られたディレクトリおよびzip/jarファイル>
                      ブートストラップ・クラス・パスの前に付加する
    -Xdiag            追加の診断メッセージを表示する
    -Xnoclassgc       クラスのガベージ・コレクションを無効にする
    -Xincgc           増分ガベージ・コレクションを有効にする
    -Xloggc:<file>    タイムスタンプが付いたファイルにGCステータスのログを記録する
    -Xbatch           バックグラウンドのコンパイルを無効にする
    -Xms<size>        Javaの初期ヒープ・サイズを設定する
    -Xmx<size>        Javaの最大ヒープ・サイズを設定する
    -Xss<size>        Javaのスレッド・スタック・サイズを設定する
    -Xprof            CPUプロファイル・データを出力する
    -Xfuture          将来のデフォルトを見越して、最も厳密なチェックを有効にする
    -Xrs              Java/VMによるOSシグナルの使用を削減する(ドキュメントを参照)
    -Xcheck:jni       JNI関数に対する追加のチェックを実行する
    -Xshare:off       共有クラスのデータを使用しようとしない
    -Xshare:auto      可能であれば共有クラスのデータを使用する(デフォルト)
    -Xshare:on        共有クラス・データの使用を必須にし、できなければ失敗する。
    -XshowSettings    すべての設定を表示して続行する
    -XshowSettings:all
                      すべての設定を表示して続行する
    -XshowSettings:vm すべてのVM関連の設定を表示して続行する
    -XshowSettings:properties
                      すべてのプロパティ設定を表示して続行する
    -XshowSettings:locale
                      すべてのロケール関連の設定を表示して続行する

-Xオプションは非標準なので、予告なく変更される場合があります。





