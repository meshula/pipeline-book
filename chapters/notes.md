
## The Importance of Look Development and Pre-production

Up front look development keeps production costs down. Technology development and pipeline development during pre-production can be tailored to the look you want to get.

The corollary to that is important to fit new development into the pipeline structure without disruption. A new shader or something might be developed, and it might be really cool, but it has to be carefully considered. If the amazing new thing is introduced, will work that has already been finalled now need to be redone?

This is an implication of the modular pipeline. If someone suddenly starts sending hex bolts down the conveyer belt when everyone is expecting screws, that's going to result in chaos.

## Random Tips

Keep the pipeline unidirectional. Don't ever send things back up the pipeline.

The tools should all have the ability to run headless, without a UI, from the command line so that the pipeline can be automated. Artists shouldn't have to sit there hitting export, export, export over and over.

The automated batch mode should have the ability to reprocess an asset type, and regenerate all downstream dependencies.

Command line interfaces should be rich enough to support what you want to do. The interface should also be rich enough to support unit tests that can exercise every processing step for validation. Besides exposing all relevant parameters, or relying on a response file, it should be possible to set up independent inputs and outputs — it should be possible that an output file not overwrite the input file.

The pipeline should have traps for bad data, incomplete processing, and error conditions that occur during processing. Errors should be caught early and reported clearly. Don't let junk get into the main pipe where it can disturb other people's work.