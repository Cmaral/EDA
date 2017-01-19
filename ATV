#include "Player.hh"

using namespace std;


/**
 * Write the name of your player and save this file
 * with the same name and .cc extension.
 */
#define PLAYER_NAME ATV_v5


struct PLAYER_NAME : public Player {


    /**
     * Factory: returns a new instance of this class.
     * Do not modify this function.
     */
    static Player* factory () {
        return new PLAYER_NAME;
    }
    

    /**
     * Attributes for your player can be defined here.
     */     
    
    struct Route {
      int steps=0; //Number of steps left to take
      queue<Dir> path; //Directions to follow to reach destination
      Pos dest; //Destination Pos
    };
        
    typedef vector< vector<bool> > Matrix;
    Route R;

    /**
     * Play method.
     * 
     * This method will be invoked once per each round.
     * You have to read the board here to place your actions
     * for this round.
     *
     */     
    virtual void play () {
      const Poquemon& p = poquemon(me());
      //Select next direction.
      Dir d = select_dir(p.pos);
      int dist = search(p, d);
      if (dist <= p.scope) attack(d);      
      //Move poquemon
      else {
	move(d);
      }
      
    }
      
    
    int search(const Poquemon& p, Dir d) {
		int dist = 1;
		Pos pIni = p.pos;
		while (dist <= p.scope) {
			pIni = dest(pIni, d);
			if (cell_type(pIni) == Wall) return p.scope+1;
			int idCell = cell_id(pIni);
			if (idCell != -1) {
				Poquemon p1 = poquemon(idCell);
				if (p1.defense <= p.attack) return dist;
			}
			++dist;
		}
		return p.scope+1;
	}
    
    //Returns opposite direction
    Dir op_dir(Dir d) {
      if (d==Top) return Bottom;
      if (d==Bottom) return Top;
      if (d==Right) return Left;
      return Right;
    }
    
      
    // Returns true is it's a correct cell (we can get the point), false otherwise.
    bool correct(Pos initial_p, Dir d) {
      const Poquemon& poq = poquemon(me());
      CType type = cell_type(dest(initial_p, d));
      if (type==Empty or type==Wall) return false;
      if (type==Stone and poq.stones==max_stone()) return false;
      if (type==Scope and poq.scope==max_scope()) return false;
      return true;
    }
    
    bool is_point(Pos p) {
      const Poquemon& poq = poquemon(me());
      CType type = cell_type(p);
      if (type==Empty or type==Wall) return false;
      if (type==Stone and poq.stones==max_stone()) return false;
      if (type==Scope and poq.scope==max_scope()) return false;
      return true;
    }
	

        
    /** Calculates closest point and stores directions in a queue of Dirs
     * **/
    queue<Dir> find_new_route(Pos& pos) {
      Pos current_pos = pos;
      
      vector<Pos> path;
      path.push_back(pos);
      queue< vector<Pos> > Q;
      Q.push(path);
      
      vector< vector<bool> > vis(rows(), vector<bool>(cols(), false));
      bool found = false;
      vis[pos.i][pos.j]=true;
      
      while (not Q.empty() and not found) {
          path = Q.front();
          Q.pop();
          Pos n_pos = path[path.size()-1];

          if (is_point(n_pos)) {
            found = true;
          } else {
            Dir D;

            //Top
            D=Top;
            Pos i_pos;
            i_pos = dest(n_pos,D);
            if (pos_ok(i_pos) and cell_type(i_pos)!=Wall and not vis[i_pos.i][i_pos.j]) {
              vector<Pos> new_path(path.begin(),path.end());

              vis[i_pos.i][i_pos.j]=true;
              new_path.push_back(i_pos);
              Q.push(new_path);
              }

            //Bottom
            D=Bottom;
            i_pos = dest(n_pos,D);
            if (pos_ok(i_pos) and cell_type(i_pos)!=Wall and not vis[i_pos.i][i_pos.j]) {
              vector<Pos> new_path(path.begin(),path.end());

              vis[i_pos.i][i_pos.j]=true;
              new_path.push_back(i_pos);
              Q.push(new_path);
              }

            //Right
            D=Right;
            i_pos = dest(n_pos,D);
            if (pos_ok(i_pos) and cell_type(i_pos)!=Wall and not vis[i_pos.i][i_pos.j]) {
              vector<Pos> new_path(path.begin(),path.end());

              vis[i_pos.i][i_pos.j]=true;
              new_path.push_back(i_pos);
              Q.push(new_path);
              }

            //Left
            D=Left;
            i_pos = dest(n_pos,D);
            if (pos_ok(i_pos) and cell_type(i_pos)!=Wall and not vis[i_pos.i][i_pos.j]) {
              vector<Pos> new_path(path.begin(),path.end());

              vis[i_pos.i][i_pos.j]=true;
              new_path.push_back(i_pos);
              Q.push(new_path);
              }
	}
	
      }
      
      queue<Dir> P;
      int s = path.size();
      
      for (int i=0; i<s; ++i) {
        Pos next = path[i];
        if ((dest(current_pos,Top)) == next) P.push(Top);
        if ((dest(current_pos,Bottom)) == next) P.push(Bottom);
        if ((dest(current_pos,Left)) == next) P.push(Left);
        if ((dest(current_pos,Right)) == next) P.push(Right);
        current_pos = next;
      }
      
      return P;
      
    }
    
  
    /**
     * Returns next direction. 
     * If a Point or Bonus is reachable, returns direction to reach it. 
     * **/
    Dir select_dir(Pos initial_p) {
        Dir d=None;
        if ((R.path).empty() or (not is_point(R.dest)) or cell_type(dest(initial_p, (R.path).front()))==Wall) {
        (R.path) = find_new_route(initial_p);
      }
      
      
      if (not (R.path).empty()) {
         d = (R.path).front();
        (R.path).pop();
      }
      
      return d;
    }
};

 /* bool no_walls(Pos initial_p, Pos point_p) {
      while (dest(initial_p, past_dir)!=point_p) {
	      if (cell_type(dest(initial_p, past_dir))==Wall) return false;
      }
      return true;
    }*/


/**
 * Do not modify the following line.
 */
RegisterPlayer(PLAYER_NAME);

