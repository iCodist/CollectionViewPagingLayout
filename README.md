# CollectionViewPagingLayout

[![License](https://img.shields.io/cocoapods/l/CollectionViewPagingLayout.svg?style=flat)](http://cocoapods.org/pods/CollectionViewPagingLayout)
![platforms](https://img.shields.io/badge/platforms-iOS-333333.svg)
[![pod](https://img.shields.io/cocoapods/v/CollectionViewPagingLayout.svg)](https://cocoapods.org/pods/CollectionViewPagingLayout)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage) 
[![Swift Package Manager compatible](https://img.shields.io/badge/Swift%20Package%20Manager-compatible-brightgreen.svg)](https://github.com/apple/swift-package-manager)

## Preview

<p float="left">
<img width="200" src="https://amir.app/git/flowlayout_preview.gif"></img>
<img width="200" src="https://amir.app/git/gallery_preview.gif"></img>
<img width="200" src="https://amir.app/git/cards_preview.gif"></img>
</p>

### SnapshotTransformView

<p float="left">
<img width="100" src="https://amir.app/git/shapes_preview/snapshot_bars.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/snapshot_chess.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/snapshot_fade.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/snapshot_grid.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/snapshot_lines.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/snapshot_puzzle.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/snapshot_space.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/snapshot_tiles.gif"></img>
</p>

### ScaleTransformView
<p float="left">
<img width="100" src="https://amir.app/git/shapes_preview/scale_cylinder.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/scale_invertedcylinder.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/scale_linear.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/scale_easein.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/scale_easeout.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/scale_rotary.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/scale_coverflow.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/scale_blur.gif"></img>
</p>

### StackTransformView

<p float="left">
<img width="100" src="https://amir.app/git/shapes_preview/stack_blur.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/stack_perspective.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/stack_reverse.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/stack_rotary.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/stack_transparent.gif"></img>
<img width="100" src="https://amir.app/git/shapes_preview/stack_vortex.gif"></img>
</p>

## About
This is a custom `UICollectionViewLayout` that gives you the ability to apply transforms easily on the cells        
by conforming your cell class to `TransformableView` protocol you will get a `progress` value and you can use it to apply any changes on your cell view.  
See [How to use](https://github.com/amirdew/CollectionViewPagingLayout#how-to-use) part for more details.  

## Installation
CollectionViewPagingLayout doesn't contain any external dependencies.

#### [CocoaPods](https://guides.cocoapods.org/using/using-cocoapods.html)

```ruby
# Podfile
use_frameworks!

target 'YOUR_TARGET_NAME' do
    pod 'CollectionViewPagingLayout'
end
```
Replace `YOUR_TARGET_NAME` and then, in the `Podfile` directory, type:

```bash
$ pod install
```

#### [Carthage](https://github.com/Carthage/Carthage)

Add this to `Cartfile`

```
github "CollectionViewPagingLayout"
```

and then, in the `Cartfile` directory, type:
```bash
$ carthage update
```

#### [Swift Package Manager](https://github.com/apple/swift-package-manager)

using Xcode:

File > Swift Packages > Add Package Dependency

#### Manually
Just add all the files under `Lib` directory to your project

## How to use
- make sure you imported the library
```swift
import CollectionViewPagingLayout
```
- Set the layout for your collection view:
(in most cases you want a paging effect so make sure you enable it)
```swift
let layout = CollectionViewPagingLayout()
collectionView.collectionViewLayout = layout
collectionView.isPagingEnabled = true // enabling paging effect
```

- Then you can set `numberOfVisibleItems`, by default it's null and that means it will load all of the cells at a time   
```swift
layout.numberOfVisibleItems = ...
```
- Now you just need to conform your `UICollectionViewCell` class to `TransformableView` and start implementing your custom transforms
> `progress` is a float value that represents the current position of your cell in the collection view.   
> When it's `0` that means the current position of the cell is exactly in the center of your collection view.   
> the value could be negative or positive and that represents the distance to the center of your collection view.   
> for instance `1` means the distance between the center of the cell and the center of your collection view is equal to your collection view width.

```swift
extension MyCollectionViewCell: TransformableView {
    func transform(progress: CGFloat) {
      ...
    }
}
```

you can start with a simple transform like this:
```swift
extension MyCollectionViewCell: TransformableView {
    func transform(progress: CGFloat) {
        let transform = CGAffineTransform(translationX: bounds.width/2 * progress, y: 0)
        let alpha = 1 - abs(progress)

        contentView.subviews.forEach { $0.transform = transform }
        contentView.alpha = alpha
    }
}
```

## Prepared Transformable Protocols

There are prepared transformables to make it easier to use this library,    
using them is very simple, you just need to conform your `UICollectionViewCell` to the prepared protocol        
and then set the options for that to customize it as you want.       
there are three types of transformables protocol at the moment `ScaleTransformView`, `SnapshotTransformView`, and `StackTransformView`.      
as you can see in the samples app these protocols are highly customizable and you can make tons of different effects with them.        
here is a simple example for `ScaleTransformView` which gives you a simple paging with scaling effect:
```swift
extension MyCollectionViewCell: ScaleTransformView {
    var scaleOptions = ScaleTransformViewOptions(
        minScale: 0.6,
        scaleRatio: 0.4,
        translationRatio: CGPoint(x: 0.66, y: 0.2),
        maxTranslationRatio: CGPoint(x: 2, y: 0),
    )
}
```
there is an options struct for each transformable where you can customize the effect, check the struct to find out what each parameter does.          
a short comment on the top of each parameter explains how you can use it.      
`ScaleTransformView` -> [`ScaleTransformViewOptions`](https://github.com/amirdew/CollectionViewPagingLayout/blob/master/Lib/Scale/ScaleTransformViewOptions.swift)     
`SnapshotTransformView` -> [`SnapshotTransformViewOptions`](https://github.com/amirdew/CollectionViewPagingLayout/blob/master/Lib/Snapshot/SnapshotTransformViewOptions.swift)     
`StackTransformView` -> [`StackTransformViewOptions`](https://github.com/amirdew/CollectionViewPagingLayout/blob/master/Lib/Stack/StackTransformViewOptions.swift)     
     
you can see some examples in the samples app for these transformables.      
check [here](https://github.com/amirdew/CollectionViewPagingLayout/tree/master/PagingLayoutSamples/Modules/Shapes/ShapeCell) to see used options for each: `/PagingLayoutSamples/Modules/Shapes/ShapeCell/`

#### Target view
You may wonder how does it figure out the view for applying transforms on, if you check each transformable protocol you can see the target views are defined for each, you can also see there is an extension to provide the default target views.    
for instance we have `ScaleTransformView.scalableView` which is the view that we apply scale transforms on, and for `UICollectionViewCell` the default view is the first subview of `contentView`:

```swift
public extension ScaleTransformView where Self: UICollectionViewCell {
    
    /// Default `scalableView` for `UICollectionViewCell` is the first subview of
    /// `contentView` or the content view itself if there is no subview
    var scalableView: UIView {
        contentView.subviews.first ?? contentView
    }
}
```
of course you can easily override this


## Customize Prepared Transformables

Yes, you can customize them or even combine them, to do that like before conform your cell class to the transformable protocol(s) and then implement `TransformableView.transform` function and call the transformable function manually, like this:     
```swift
extension LayoutTypeCollectionViewCell: ScaleTransformView {
    
    func transform(progress: CGFloat) {
        applyScaleTransform(progress: progress)
        // customize views here, like this:
        titleLabel.alpha = 1 - abs(progress)
        subtitleLabel.alpha = titleLabel.alpha
    }

}
```
as you can see `applyScaleTransform` applies the scale transforms and right after that we change the alpha for `titleLabel` and `subtitleLabel`.        
to find the public function(s) for each tansformable check the protocol definition.      

## Other features

You can control the current page by following public functions of `CollectionViewPagingLayout`:
- `func setCurrentPage(_ page: Int, animated: Bool = true)`
- `func goToNextPage(animated: Bool = true)`
- `func goToPreviousPage(animated: Bool = true)`       

these are safe wrappers for setting `ContentOffset` of `UICollectionview`        
you can also get current page by a public variable `CollectionViewPagingLayout.currentPage` or listen to the changes by setting `CollectionViewPagingLayout.delegate`:      

```swift
public protocol CollectionViewPagingLayoutDelegate: class {
    func onCurrentPageChanged(layout: CollectionViewPagingLayout, currentPage: Int)
}
```


## Limitations
You need to specify the number of visible cells since this layout gives you the flexibility to show the next and previous cells.   
By default, the layout loads all of the cells in the collection view frame and that means it keeps all of them in memory.
You can specify the number of cells that you need to show at a time by considering your design.


## License

CollectionViewPagingLayout is available under the MIT license. See LICENSE file for more info.
