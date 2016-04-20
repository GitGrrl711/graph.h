# graph.h

#ifndef _GRAPH_H_
#define _GRAPH_H_
 
#include <cstddef>
#include <iostream>
#include <list>
#include <vector> // Vertex container
#include <map> // Edge container
#include <utility>
 
 //Thomas Step, Riya Santra 
 
////////////////////////////////////////////////////////////////////////////////
/// A generic adjacency-list graph where each vertex stores a VertexProperty and
/// each edge stores an EdgeProperty.
////////////////////////////////////////////////////////////////////////////////
template<typename VertexProperty, typename EdgeProperty>
class graph {
 
  // The vertex and edge classes are forward-declared to allow their use in the
  // public section below. Their definitions follow in the private section
  // afterward.
  class vertex;
  class edge;
 
  public:
 
    // Required public types
 
    /// Unique vertex identifier
    typedef size_t vertex_descriptor;
 
    /// Unique edge identifier represents pair of vertex descriptors
    typedef std::pair<size_t, size_t> edge_descriptor;
 
    ///@done Choose a container for the vertices. It should contain "vertex*" or
    ///      shared_ptr<vertex>.
    typedef std::map<vertex_descriptor, vertex*> MyVertexContainer; // Using vector, plan on making the vd/ID the index
 
    ///@todo Choose a container for the edges. It should contain "edge*" or
    ///      shared_ptr<edge>.
    /// example:
    typedef std::map<edge_descriptor, edge*> MyEdgeContainer; // Make the unique key the ed, then the value type points to the edge
 
    ///@todo Choose a container for the adjacency lists. It should contain
    ///      "edge*" or shared_ptr<edge>.
    /// example:
    typedef std::map<edge_descriptor, edge*> MyAdjEdgeContainer; // Maybe dictionary
 
    // Vertex iterators
    typedef typename MyVertexContainer::iterator vertex_iterator;
    typedef typename MyVertexContainer::const_iterator const_vertex_iterator;
 
    // Edge iterators
    typedef typename MyEdgeContainer::iterator edge_iterator;
    typedef typename MyEdgeContainer::const_iterator const_edge_iterator;
 
    // Adjacency list iterators
    typedef typename MyAdjEdgeContainer::iterator adj_edge_iterator;
    typedef typename MyAdjEdgeContainer::const_iterator const_adj_edge_iterator;
 
    // Required graph operations
 
    ///@todo Define constructor/destructor
    graph() = default;
    ~graph() = default;
 
    graph(const graph&) = delete;             ///< Copy is disabled.
    graph& operator=(const graph&) = delete;  ///< Copy is disabled.
 
    ///@todo Define vertex iterator operations
    vertex_iterator vertices_begin() {
      return mvc.begin();
    }
    const_vertex_iterator vertices_cbegin() const {
      return mvc.cbegin();
    }
    vertex_iterator vertices_end() {
      return mvc.end();
    }
    const_vertex_iterator vertices_cend() const {
      return mvc.cend();
    }
 
    ///@todo Define edge iterator operations
    edge_iterator edges_begin() {
      return mec.begin();
    };
    const_edge_iterator edges_cbegin() const {
      return mec.cbegin();
    };
    edge_iterator edges_end() {
      return mec.end();
    };
    const_edge_iterator edges_cend() const {
      return mec.cend();
    };
 
    ///@todo Define accessors
    size_t num_vertices() const;
    size_t num_edges() const;
    vertex_iterator find_vertex(vertex_descriptor);
    const_vertex_iterator find_vertex(vertex_descriptor) const;
    edge_iterator find_edge(edge_descriptor);
    const_edge_iterator find_edge(edge_descriptor) const;
 
    ///@todo Define modifiers
    vertex_descriptor insert_vertex(const VertexProperty& val) {
      vertex* myVertex = new vertex(0, val);
      std::pair<vertex_iterator, bool> vi = mvc.insert(std::pair<vertex_descriptor, vertex*>(0,myVertex));
      vertex_descriptor vd = vi.first->first;
      return vd;
    };
   
    edge_descriptor insert_edge(vertex_descriptor vd1, vertex_descriptor vd2,
        const EdgeProperty& ep) {
      edge* myEdge = edge(vd1, vd2, ep); // Creating a new edge with the given info
     // std::pair<edge_iterator, bool> vi = mec.insert(std::pair<edge_descriptor, edge*>(0,myEdge));
	  edge_descriptor ed = edge_descriptor(vd1, vd2);
      return ed;
	  //return myEdge;
    };
       
    void insert_edge_undirected(vertex_descriptor, vertex_descriptor,
        const EdgeProperty&);
       
    void erase_vertex(vertex_descriptor); // Do we need to shift the vd's of all other vertices or just leave a hole?
   
    void erase_edge(edge_descriptor);
   
    void clear();
 
    // Friend declarations for input/output.
    template<typename V, typename E>
    friend std::istream& operator>>(std::istream&, graph<V, E>&);
    template<typename V, typename E>
    friend std::ostream& operator<<(std::ostream&, const graph<V, E>&);
 
  private:
 
    // Required internal classes
	 MyVertexContainer mvc;
     MyEdgeContainer mec;
 
    ////////////////////////////////////////////////////////////////////////////
    /// Vertices represent the nodes in the graph.
    ///
    /// @todo Specify the internal state of the vertex class and define all of
    ///       the functions declared in its interface.
    ////////////////////////////////////////////////////////////////////////////
    class vertex {
 
      public:
 
        ///@todo Define constructor
        vertex(vertex_descriptor vd, const VertexProperty& v) : id(0), val(0) { // Default assignment of values
          vd = vd; // If these are given in the constructor, use them
          val = v;
        };
 
        ///@todo Define iterator operations
        adj_edge_iterator begin();
        const_adj_edge_iterator cbegin() const;
        adj_edge_iterator end();
        const_adj_edge_iterator cend() const;
 
        ///@maybe Define accessor operations
        const vertex_descriptor descriptor() const {
          return id;
        };
       
        VertexProperty& property() {
          VertexProperty& valref = val;
          return valref;
        };
       
        const VertexProperty& property() const {
          VertexProperty& valref = val;
          return valref;
        };
 
      private:
 
        ///@maybe Specify the internal state of a vertex.
        vertex_descriptor id; // This is the vertex's ID
        VertexProperty val; // This is the value stored in the vertex
    };
 
 
    ////////////////////////////////////////////////////////////////////////////
    /// Edges represent the connections between nodes in the graph.
    ///
    /// @todo Specify the internal state of the edge class and define all of the
    ///       functions declared in its interface.
    ////////////////////////////////////////////////////////////////////////////
    class edge {
 
      public:
 
        ///@todo Define constructor
        edge(vertex_descriptor vd1, vertex_descriptor vd2, const EdgeProperty& ep) {
          ed.first = vd1;
          ed.second = vd2;
          dist = ep;
        };
 
        ///@todo Define accessor operations
        const vertex_descriptor source() const;
        const vertex_descriptor target() const;
       
        const edge_descriptor descriptor() const {
          return ed;
        };
       
        EdgeProperty& property();
        const EdgeProperty& property() const;
 
      private:
 
        ///@maybe Specify the internal state of an edge.
        edge_descriptor ed; // Which 2 vertices does this edge connect?
        EdgeProperty dist; // The edge property
    };
   
    // Containers for the graph's vertices, edges, and adjacent edges
   
   
   
};
 
///@todo Define io operations for the graph.
template<typename V, typename E>
std::istream& operator>>(std::istream&, graph<V, E>&);
template<typename V, typename E>
std::ostream& operator<<(std::ostream&, const graph<V, E>&);
 
#endif
