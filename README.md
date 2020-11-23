using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp2
{
    class Program
    {
        private static IDictionary<string, TreeNode> islands = new Dictionary<string, TreeNode>();
        private static int[] satirDeltas = new int[] { -1, 0 };
        private static int[] sutunDeltas = new int[] { 0, -1 };
        private static int[,] mat = new int[,] {
            { 1, 1, 0, 0, 0 },
            { 0, 1, 0, 0, 1 },
            { 1, 1, 0, 1, 1 },
            { 0, 0, 0, 0, 0 },
            { 1, 0, 1, 0, 1 }
        };

        private class TreeNode
        {
            public TreeNode(String key)
            {
                Key = key;
                Children = new List<TreeNode>();
            }
            public List<TreeNode> Children { get; set; }
            public string Key { get; }
        }

        static void Main(string[] args)
        {
            var islandIndex = 0;

            for (int satir = 0; satir < 5; satir++)
            {
                for (int sutun = 0; sutun < 5; sutun++)
                {
                    var value = mat[satir, sutun];
                    if (value == 0)
                    {
                        continue;
                    }

                    Register(islandIndex, satir, sutun);
                    islandIndex++;
                }
            }

        }

        private static void Register(int islandIndex, int satir, int sutun)
        {
            var key = $"{satir} - {sutun}";
            TreeNode island = new TreeNode(key);

            for (int i = 0; i < satirDeltas.Length; i++)
            {
                var satirDelta = satirDeltas[i];
                var sutunDelta = sutunDeltas[i];

                var neighbourSatir = satir + satirDelta;
                var neighbourSutun = sutun + sutunDelta;
                var neigbourKey = $"{neighbourSatir} - {neighbourSutun}";

                if (!islands.ContainsKey(neigbourKey))
                {
                    continue;
                }

                var neigbourIsland = islands[neigbourKey];
                island.Children.Add(neigbourIsland);
                islands.Remove(neigbourKey);
            }

            islands[key] = island;
        }
    }
}
