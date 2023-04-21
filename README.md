# swiftui-keyboardheight

```swift
import Foundation
import SwiftUI
import Combine

fileprivate extension Publishers {
    static var keyboardHeight: AnyPublisher<CGFloat, Never> {
        let willShow = NotificationCenter.default.publisher(for: UIResponder.keyboardWillShowNotification)
            .map { ($0.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? CGRect)?.height ?? 0 }
        let willHide = NotificationCenter.default.publisher(for: UIResponder.keyboardWillHideNotification)
            .map { _ in CGFloat(0) }
        return MergeMany(willShow, willHide)
            .eraseToAnyPublisher()
    }
}

extension View {
    
    func keyboardHeight(completion: @escaping(CGFloat) -> Void) -> some View {
        self
            .onReceive(Publishers.keyboardHeight) { keyboardHeight in
                completion(keyboardHeight)
            }
    }
    
}

```
