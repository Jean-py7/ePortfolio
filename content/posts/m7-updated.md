```swift
import SwiftUI

struct Shipment: Decodable, Identifiable {
    let id: String
    let status: String
}

@MainActor
final class ShipmentViewModel: ObservableObject {
    @Published var results: [Shipment] = []
    @Published var errorText: String?

    func search(tracking: String) async {
        guard !tracking.isEmpty else { results = []; return }
        do {
            let url = URL(string: "https://api.example.com/shipments/\(tracking)")!
            let (data, _) = try await URLSession.shared.data(from: url)
            results = try JSONDecoder().decode([Shipment].self, from: data)
            errorText = nil
        } catch {
            errorText = "Could not load tracking info."
        }
    }
}


