# RWA-Dijkstra

//Classe Principal

import java.util.PriorityQueue;
import java.util.Random;

public class ExecutorRWA {
	private static final Graph.Edge[] GRAFO = { new Graph.Edge("SEA", "SFA", 1072), new Graph.Edge("SEA", "SLC", 1086),
			new Graph.Edge("SEA", "CHI", 2731), new Graph.Edge("SFA", "SEA", 1072), new Graph.Edge("SFA", "LOA", 535),
			new Graph.Edge("LOA", "SFA", 535), new Graph.Edge("LOA", "SLC", 932), new Graph.Edge("LOA", "ELP", 1123),
			new Graph.Edge("SLC", "SEA", 1086), new Graph.Edge("SLC", "LOA", 932), new Graph.Edge("SLC", "DEN", 601),
			new Graph.Edge("CHI", "SEA", 2731), new Graph.Edge("CHI", "DEN", 1438), new Graph.Edge("CHI", "KAN", 651),
			new Graph.Edge("CHI", "CLE", 474), new Graph.Edge("DEN", "SLC", 601), new Graph.Edge("DEN", "CHI", 1438),
			new Graph.Edge("DEN", "ELP", 878), new Graph.Edge("DEN", "KAN", 867), new Graph.Edge("ELP", "LOA", 1123),
			new Graph.Edge("ELP", "DEN", 878), new Graph.Edge("ELP", "HOU", 1053), new Graph.Edge("KAN", "CHI", 651),
			new Graph.Edge("KAN", "DEN", 867), new Graph.Edge("KAN", "DAL", 699), new Graph.Edge("KAN", "NSH", 758),
			new Graph.Edge("HOU", "ELP", 1053), new Graph.Edge("HOU", "DAL", 355), new Graph.Edge("DAL", "KAN", 699),
			new Graph.Edge("DAL", "HOU", 355), new Graph.Edge("DAL", "ATL", 1133), new Graph.Edge("ATL", "DAL", 1133),
			new Graph.Edge("ATL", "NSH", 337), new Graph.Edge("ATL", "TAM", 644), new Graph.Edge("NSH", "KAN", 758),
			new Graph.Edge("NSH", "ATL", 337), new Graph.Edge("NSH", "CHA", 519), new Graph.Edge("NSH", "CLE", 708),
			new Graph.Edge("TAM", "ATL", 644), new Graph.Edge("TAM", "MIA", 321), new Graph.Edge("MIA", "TAM", 321),
			new Graph.Edge("MIA", "CHA", 1027), new Graph.Edge("CHA", "NSH", 519), new Graph.Edge("CHA", "MIA", 1027),
			new Graph.Edge("CHA", "PHI", 707), new Graph.Edge("PHI", "CHA", 707), new Graph.Edge("PHI", "NYC", 143),
			new Graph.Edge("CLE", "CHI", 474), new Graph.Edge("CLE", "NSH", 708), new Graph.Edge("CLE", "BOS", 886),
			new Graph.Edge("CLE", "NYC", 652), new Graph.Edge("BOS", "CLE", 886), new Graph.Edge("BOS", "NYC", 297),
			new Graph.Edge("NYC", "PHI", 143), new Graph.Edge("NYC", "CLE", 652), new Graph.Edge("NYC", "BOS", 297),

	};

	public static void main(String[] args) {

		String cidades[] = { "SEA", "SFA", "LOA", "SLC", "CHI", "DEN", "ELP", "KAN", "HOU", "DAL", "ATL", "NSH", "TAM",
				"MIA", "CHA", "PHI", "CLE", "BOS", "NYC" };

		Random random = new Random();

		String INICIO = cidades[random.nextInt(cidades.length)];
		String FIM = cidades[random.nextInt(cidades.length)];

		System.out.println("Cidade inicial:" + INICIO + " - Cidade final:" + FIM);

		Graph g = new Graph(GRAFO);
		g.dijkstra(INICIO);
		g.printPath(FIM);

		//g.printAllPaths();
	}
}
/*
 * double next = 0; int counter = 0;
 * 
 * PriorityQueue established = new PriorityQueue<>();
 * 
 * 
 * int contador = 0; int N = 10000; while (contador < N){
 * 
 * if (established.isEmpty()) { establish(lambda, mi, Grafo, link, W);
 * contador++; } }
 * 
 * 
 * 
 * } void establish(int lambda, double mi, ) }
 */
 
 
 
 // Classe do Dijkstra
 
 
 import java.io.*;
import java.util.*;

class Graph {
  private final Map<String, Vertex> grafo; // mapping of vertex names to Vertex objects, built from a set of Edges
  
  /** One edge of the graph (only used by Graph constructor) */
  public static class Edge {
    public final String v1, v2;
    public final int dist;
    public Edge(String v1, String v2, int dist) {
      this.v1 = v1;
      this.v2 = v2;
      this.dist = dist;
    }
  }
  
  /** One vertex of the graph, complete with mappings to neighbouring vertices */
  public static class Vertex implements Comparable<Vertex>{
	  
    public final String name;
    public int dist = Integer.MAX_VALUE; 
    public Vertex previous = null;
    public final Map<Vertex, Integer> neighbours = new HashMap<>();
    
    public Vertex(String name)
    {
      this.name = name;
    }
    
    private void printPath()
    {
      if (this == this.previous)
      {
        System.out.printf("%s", this.name);
      }
      else if (this.previous == null)
      {
        System.out.printf("%s(unreached)", this.name);
      }
      else
      {
        this.previous.printPath();
        System.out.printf(" -> %s(%d)", this.name, this.dist);
      }
    }
    
    public int compareTo(Vertex other)
    {
      if (dist == other.dist)
        return name.compareTo(other.name);
      
      return Integer.compare(dist, other.dist);
    }
    
    @Override public String toString()
    {
      return "(" + name + ", " + dist + ")";
    }
  }
  
  /** Builds a graph from a set of edges */
  public Graph(Edge[] edges) {
    grafo = new HashMap<>(edges.length);
    
    //one pass to find all vertices
    for (Edge e : edges) {
      if (!grafo.containsKey(e.v1)) grafo.put(e.v1, new Vertex(e.v1));
      if (!grafo.containsKey(e.v2)) grafo.put(e.v2, new Vertex(e.v2));
    }
    
    //another pass to set neighbouring vertices
    for (Edge e : edges) {
      grafo.get(e.v1).neighbours.put(grafo.get(e.v2), e.dist);
      //graph.get(e.v2).neighbours.put(graph.get(e.v1), e.dist); // also do this for an undirected graph
    }
  }
  
  /** Runs dijkstra using a specified source vertex */ 
  public void dijkstra(String startName) {
    if (!grafo.containsKey(startName)) {
      System.err.printf("Graph doesn't contain start vertex \"%s\"\n", startName);
      return;
    }
    final Vertex source = grafo.get(startName);
    NavigableSet<Vertex> q = new TreeSet<>();
    
    // set-up vertices
    for (Vertex v : grafo.values()) {
      v.previous = v == source ? source : null;
      v.dist = v == source ? 0 : Integer.MAX_VALUE;
      q.add(v);
    }
    
    dijkstra(q);
  }
  
  /** Implementation of dijkstra's algorithm using a binary heap. */
  private void dijkstra(final NavigableSet<Vertex> q) {      
    Vertex u, v;
    while (!q.isEmpty()) {
      
      u = q.pollFirst(); // vertex with shortest distance (first iteration will return source)
      if (u.dist == Integer.MAX_VALUE) break; // we can ignore u (and any other remaining vertices) since they are unreachable
      
      //look at distances to each neighbour
      for (Map.Entry<Vertex, Integer> a : u.neighbours.entrySet()) {
        v = a.getKey(); //the neighbour in this iteration
        
        final int alternateDist = u.dist + a.getValue();
        if (alternateDist < v.dist) { // shorter path to neighbour found
          q.remove(v);
          v.dist = alternateDist;
          v.previous = u;
          q.add(v);
        } 
      }
    }
  }
  
  /** Prints a path from the source to the specified vertex */
  public void printPath(String nomeFinal) {
    grafo.get(nomeFinal).printPath();
    System.out.println();
  }
  /** Prints the path from the source to every vertex (output order is not guaranteed) */
  public void printAllPaths() {
    for (Vertex v : grafo.values()) {
      v.printPath();
      System.out.println();
    }
  }
}
