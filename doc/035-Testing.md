# Testing blocks

Blocks can be tested using the PySys testing framework. This is included in the Apama installation, along with extensions for using Apama with PySys. Built on top of the Apama extensions is a framework to test blocks. Refer to the [Apama Python API documentation](http://www.apamacommunity.com/documents/10.3.1.1/apama_10.3.1.1_webhelp/pydoc/).

The samples include tests. The `pysystestproject.xml` configuration relies on the environment variable `ANALYTICS_BUILDER_SDK` being set to the location of the block SDK using an absolute path. PySys tests should contain a `run.py` with a class that extends `apama.analyticsbuilder.basetest:AnalyticsBuilderBaseTest`. In the `execute` method of the test, start a correlator with the `self.startAnalyticsBuilderCorrelator()` method. This starts a correlator, injects the Analytics Builder framework into it, and returns a `CorrelatorHelper` object. Provide a `blockSourceDir` parameter with the path to the source of the blocks, typically within the project tree (use `self.project.SOURCE` from the supplied `pysysproject.xml` file). Then, create a model to test the block with the `self.createTestModel('<blockname>')` method where `<blockname>` is the fully qualified name of the block (this method defaults to using the last correlator started by `startAnalyticsBuilderCorrelator`, or this can be supplied). This results in a model being activated in the correlator with an input and output connected to every input and output of the block, and an identifier of the model is returned. The block can be exercised by sending events created by the `self.inputEvent` method, for a given block input identifier.

Both `createTestModel` and `inputEvent` take an optional argument: `modelId` - an identifier of the model. If an identifier is not specified, `createModel` will use the identifiers `model_0` and upwards, and `inputEvent` will use `model_0` (that is, the first created model). 

`createTestModel` also takes optional arguments:

* `inputs` - a map of the types of inputs, from input identifier to the type of the input. If the entry is `None` or an empty string, then no input is connected. Inputs will default to the `float` type if there is no key for an input identifier.

Note that the correlator started by `startAnalyticsBuilderCorrelator` is externally clocked, and inputs will be processed 0.1 seconds (in correlator time) after the event has been received. If multiple values for the same input are received at the same correlator time, it is undefined which will be processed. Use the `timestamp` method to generate a time pseudo-event.

For example, a simple test is:

```python
class PySysTest(AnalyticsBuilderBaseTest):
    def execute(self):
        correlator = self.startAnalyticsBuilderCorrelator(
                     blockSourceDir=f'{self.project.SOURCE}/blocks/')
        modelId = self.createTestModel('apamax.analyticsbuilder.samples.Difference')
        self.sendEventStrings(correlator,
                              self.timestamp(1),
                              self.inputEvent('value1', 12.25, id = modelId),
                              self.timestamp(2),
                              self.inputEvent('value2', 7.75, id = modelId),
                              self.timestamp(2.1))

    def validate(self):
        self.assertGrep('output.evt', expr=self.outputExpr('absoluteDifference', 4.5))

```

Points to be aware of:

* If using `sendEvents` (that is, from a file), include a `&FLUSHING(5)` line at the start of the file. This ensures events are processed completely to avoid race conditions. `self.sendEventStrings` will do this automatically.
* If using `assertGrep` on floating point numbers, beware of rounding errors (for example, a result of `4.6` may be rendered as `4.599999999997` in an event file due to rounding errors).

For examples, see the tests in the **samples/tests** directory.

[< Prev: Building a block into an extension](030-BuildingExtensions.md) | [Contents](000-contents.md) | [Next: Parameters, block startup and error handling >](040-Parameters.md) 
