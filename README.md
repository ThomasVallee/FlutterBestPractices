# Flutter Best Practices & Must Do's

When working on an internal flutter project, please refer to this document. Failure to follow some of the instructions in this file can lead to a rejected PR (pull request) / a refractor request.

*Created & Maintained by Thomas Vallee & Micah Meadows*

# File Structure

Following the project's file structure is an important aspect of a clean architecture.

## Create files and folders

 - Files should follow the **snake_case.dart** naming convention
 - New assets should be placed in their respective folder inside the /assets directory. ie: (fonts, images, icons).
 - Business logic related files should be created inside the data directory
 - Refer to the Resources directory for the theme or constant related elements. 
 

# BloC Architecture 

We will be using the **flutter_bloc** library, for all of our internal projects, if you are not familiar with this library refer to the following knowledge resources:

- https://pub.dev/packages/flutter_bloc
- https://medium.com/flutter-community/flutter-bloc-for-beginners-839e22adb9f5


## Creating a new Cubit

Depending on the business logic needed, creating a new cubit may require up to 4 files, example_cubit.dart, example_state.dart, example_repository.dart & I_example_repository.dart (interface).

## Cubit & State classes
Initializing Cubit class
> `import /resources/repositories/example_repository.dart` 
> `part "example_state.dart"` 
> `class ExampleCubit extends Cubit<ExampleState>{}`

 Initializing State class 
> `part "example_cubit.dart"` 
> `@immutable`
> `abstract class ExampleState {}`
> `class ExampleInital extends ExampleState {}`
- Cubits must take their respective repository as an argument
- Do not implement View logic inside cubit classes
- Methods should be placed in the respective repository class & called from cubit
- Cubit methods do not return values, instead they emit state with a value `emit(const ExampleInital(value))` 
- If Cubit is needed globally add it to `MultiBlocProvider` in main.dart. If it is only needed locally provide the `BlocProvider`above the respected widget in the widget tree.


## Repository classes
**The repository class' holds the Business logic of the app, while the cubits just send the values to the Views.**
- Repositories must have an interface class to implement
- When adding the repository to the `MultiRepositoryProvider` in main, you MUST provide the interface class.


# Widgets

- Only create Stateful Widgets when needed, for all other implementations use Stateless Widgets.
- Always extract Widgets to a separate class file when possible. This reduces the load when SetState is called.
- Add common use Widgets to the /common directory for reusability.
- Try to reduce the use of SetState as much as possible.
- Use cubit state updates to update view logic where possible using `BlocBuilder<ExampleCubit, ExampleState>(builder: (context, exampleState) {});`
- Use **Flex** based widgets when applicable. (Flex, Row, Column, Expanded)...
- Use if conditions instead of conditional expressions inside widget logic were possible (especially if the value can be null)
- Avoid adding large operations inside the build() method
