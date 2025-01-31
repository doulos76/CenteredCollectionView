# Usage

How to install and use this module.

1. [Installation](#installation)
1. Usage
    - [Storyboard Usage](#storyboard-usage)
    - [Programmatic Usage](#programmatic-usage)
1. [Scroll to Edge Effect](#scroll-to-edge-effect)


## Installation

CenteredCollectionView is available through [CocoaPods](http://cocoapods.org) and [Carthage](https://github.com/Carthage/Carthage).

To install it with **Cocoapods**, add the following line to your `Podfile`:
```ruby
pod "CenteredCollectionView"
```

To install it with **Carthage**, add the following line to your `Cartfile`:
```
github "BenEmdon/CenteredCollectionView"
```

## Storyboard Usage
1. To start off lets add a `UICollectionView` to our controller, and lay it out however you want it.
  ![AddCollectionView](/.github/AddCollectionView.gif)
1. Next lets set the `UICollectionView`'s custom layout to be `CenteredCollectionViewFlowLayout`.
  ![GiveFlowLayout](/.github/GiveFlowLayout.gif)
1. Next we can optionally layout the prototype item. **Note that this doesn't determine the item's size.**
  ![MakeItemBig](/.github/MakeItemBig.gif)

1. Make sure the collection view cell reuse identifier is set.
  ![CellIdentifier](/.github/CellIdentifier.png)

1. Create an `IBOutlet` for the collection view.
  ![IBOutlet](/.github/IBOutlet.gif)

1. Next lets dive in to the code use:
  ```swift
  class ViewController: UIViewController {

  	@IBOutlet weak var collectionView: UICollectionView!

  	// The width of each cell with respect to the screen.
  	// Can be a constant or a percentage.
  	let cellPercentWidth: CGFloat = 0.7

  	// A reference to the `CenteredCollectionViewFlowLayout`.
  	// Must be aquired from the IBOutlet collectionView.
  	var centeredCollectionViewFlowLayout: CenteredCollectionViewFlowLayout!

  	override func viewDidLoad() {
  		super.viewDidLoad()

  		// Get the reference to the `CenteredCollectionViewFlowLayout` (REQUIRED STEP)
  		centeredCollectionViewFlowLayout = collectionView.collectionViewLayout as! CenteredCollectionViewFlowLayout

  		// Modify the collectionView's decelerationRate (REQUIRED STEP)
  		collectionView.decelerationRate = UIScrollViewDecelerationRateFast

  		// Assign delegate and data source
  		collectionView.delegate = self
  		collectionView.dataSource = self

  		// Configure the required item size (REQUIRED STEP)
  		centeredCollectionViewFlowLayout.itemSize = CGSize(
  			width: view.bounds.width * cellPercentWidth,
  			height: view.bounds.height * cellPercentWidth * cellPercentWidth
  		)

  		// Configure the optional inter item spacing (OPTIONAL STEP)
  		centeredCollectionViewFlowLayout.minimumLineSpacing = 20

  		// Get rid of scrolling indicators
  		collectionView.showsVerticalScrollIndicator = false
  		collectionView.showsHorizontalScrollIndicator = false
  	}
  }
  ```

## Programmatic Usage
```Swift
class ViewController: UIViewController {

  let centeredCollectionViewFlowLayout = CenteredCollectionViewFlowLayout()
  let collectionView: UICollectionView

  override init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: Bundle?) {
    // Initialize the collectonView with the `CenteredCollectionViewFlowLayout` (REQUIRED STEP)
    collectionView = UICollectionView(centeredCollectionViewFlowLayout: centeredCollectionViewFlowLayout)
    super.init(nibName: nibNameOrNil, bundle: nibBundleOrNil)
  }

  override func viewDidLoad() {
    super.viewDidLoad()

    // Assign delegate and data source
    collectionView.delegate = self
    collectionView.dataSource = self

    // Layout subviews (not shown)
    ...

    // Register collection cells (REQUIRED STEP)
    collectionView.register(UICollectionViewCell.self, forCellWithReuseIdentifier: String(describing: UICollectionViewCell.self))

    // Configure the required item size (REQUIRED STEP)
    centeredCollectionViewFlowLayout.itemSize = CGSize(
      width: view.bounds.width * cellPercentWidth,
      height: view.bounds.height * cellPercentWidth * cellPercentWidth
    )

    // Configure the optional inter item spacing (OPTIONAL STEP)
    centeredCollectionViewFlowLayout.minimumLineSpacing = 20

    // Get rid of scrolling indicators
    collectionView.showsVerticalScrollIndicator = false
    collectionView.showsHorizontalScrollIndicator = false
  }
}

// Delegate and datasource extensions
...

```

## Scroll to Edge Effect
![scrollToEdgeEnabled](/.github/ScrollToEdge.gif)

_Scrolling to an Edge on Touch 🎡_

Heres how you could trigger a scroll animation when a touch happens on an item that isn't the `currentCenteredPage`:

```swift
extension ViewController: UICollectionViewDelegate {
  func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {

    // check if the currentCenteredPage is not the page that was touched
    let currentCenteredPage = centeredCollectionViewFlowLayout.currentCenteredPage
    if currentCenteredPage != indexPath.row {
      // trigger a scrollToPage(index: animated:)
      centeredCollectionViewFlowLayout.scrollToPage(index: indexPath.row, animated: true)
    }
  }
}
```
