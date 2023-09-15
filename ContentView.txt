//
//  ContentView.swift
//  Desafio Mapa
//
//  Created by Student11 on 06/09/23.
//

import SwiftUI
import Foundation
import MapKit
struct Location: Identifiable{
    var id = UUID()
    var name: String
    var coord: CLLocationCoordinate2D
    var flag: String
    var description: String
}

struct ContentView: View {
    
    @State private var modo2 = false
    @State private var modo3 = false
    @State private var nome = "Brasil"
    @State private var region = MKCoordinateRegion(center: CLLocationCoordinate2D(latitude: 51.507222,longitude: -0.1275), span:MKCoordinateSpan(latitudeDelta:0.5, longitudeDelta:0.5))
    @State private var historia = "Brasil"
    @State private var list: [Location] = [Location (name: "Brasil", coord: CLLocationCoordinate2D(latitude: -15.7801, longitude: -47.9292), flag: "https://www.gov.br/planalto/pt-br/conheca-a-presidencia/acervo/simbolos-nacionais/bandeira/bandeira-nacional-brasil.jpg", description: "O Brasil, um vasto país sul-americano, estende-se da Bacia Amazônica, no norte, até os vinhedos e as gigantescas Cataratas do Iguaçu, no sul. O Rio de Janeiro, simbolizado pela sua estátua de 38 metros de altura do Cristo Redentor, situada no topo do Corcovado, é famoso pelas movimentadas praias de Copacabana e Ipanema, bem como pelo imenso e animado Carnaval, com desfiles de carros alegóricos, fantasias extravagantes e samba."), Location (name: "Estados Unidos da America", coord: CLLocationCoordinate2D(latitude: 40.730610, longitude: -73.935242), flag: "https://s3.static.brasilescola.uol.com.br/be/conteudo/images/estados-unidos.jpg", description: "Os EUA são um país com 50 estados que cobrem uma vasta faixa da América do Norte, com o Alasca ao noroeste e o Havaí no Oceano Pacífico, estendendo a presença do país. As principais cidades da costa atlântica são Nova York, um centro financeiro e cultural global, e a capital, Washington, DC. Chicago, uma metrópole do centro-oeste, é conhecida por sua importante arquitetura, enquanto Los Angeles, na costa oeste, é famosa pelas produções cinematográficas de Hollywood."), Location (name: "Alemanha", coord: CLLocationCoordinate2D(latitude: 52.5186, longitude: 13.4081), flag: "https://upload.wikimedia.org/wikipedia/commons/thumb/b/ba/Flag_of_Germany.svg/255px-Flag_of_Germany.svg.png", description: "A Alemanha é um país situado na Europa Ocidental com uma paisagem de florestas, rios, cordilheiras e praias do Mar do Norte. A nação tem mais de 2 milênios de história. Berlim, a capital, abriga cenários artísticos e de vida noturna, o Portão de Brandemburgo e muitos locais relacionados à Segunda Guerra Mundial. Munique é conhecida pela Oktoberfest e pelos beer halls, entre eles o Hofbräuhaus, do século XVI. Frankfurt, com seus arranha-céus, abriga o Banco Central Europeu."), Location (name: "Africa do Sul" , coord: CLLocationCoordinate2D(latitude: -26.2044, longitude: 28.0416), flag: "https://dwpt1kkww6vki.cloudfront.net/img/drapeau/120/192.png", description: "A África do Sul é um país situado na extremidade sul do continente africano e marcado por vários ecossistemas diferentes. O Parque Nacional Kruger, um destino para safári no interior do país, é repleto de animais de grande porte. A província de Cabo Ocidental tem praias, vinícolas exuberantes perto de Stellenbosch e Paarl, colinas escarpadas no Cabo da Boa Esperança, florestas e lagoas ao longo da Tuinroete (rota dos jardins) e a Cidade do Cabo, que fica ao pé da montanha da Mesa, de topo achatado.")]
    
    var body: some View {
        VStack {
            Text("World Map")
                .font(.largeTitle)
                .fontWeight(.bold)
            
            Text(nome)
            Map(coordinateRegion:$region, annotationItems: list){ p in
                MapAnnotation(coordinate: p.coord){
                    Image(systemName: "dot.circle.and.hand.point.up.left.fill")
                        .onTapGesture {
                            modo2.toggle()
                            historia = "\(p.description)"
                            
                        } .sheet(isPresented: $modo2, onDismiss: didDismiss) {
                            VStack {
                            Text(historia)                            }
                            }
                    
                }
            }
                .frame(width: 700, height: 560)
            ScrollView(.horizontal, showsIndicators: false) {
                HStack{
                    ForEach(list) { list in
                        Button (action: {
                            modo3.toggle()
                            nome = "\(list.name)"
                            region.center = list.coord
                            
                        }) {
                            
                            ZStack {
                                VStack {
                                    
                                    
                                    AsyncImage(url: URL (string: "\(list.flag)")) { image in
                                        image.resizable()
                                            .scaledToFit()
                                            .frame(width: 100, height: 50)
                                            .padding()
                                    } placeholder: {
                                        ProgressView()
                                    }
                                    Text("\(list.name)  ")
                                        .foregroundColor(.black)
                                        .background(Color.blue)
                                        .cornerRadius(15)
                                        
                                }
                                Spacer()
                            }
                            Spacer()
                        }
        
                    }
                    


                    /* .sheet(isPresented: $modo3, onDismiss: didDismiss) {
                     VStack {
                     Text("Edilson")
                     Text("Almeida")
                     Text("hackatruck.com.br")
                     Text("edilsonalmeida_")
                     }
                     }*/
                    
                    
                }
            }
        }
    }
    func didDismiss(){
        
    }
}


struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
